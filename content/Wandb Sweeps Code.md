The following code comes from: 
https://github.com/wandb/sweeps/blob/master/src/sweeps/params.py

```
"""Hyperparameter search parameters."""
import logging
import operator
import random
from copy import deepcopy
from functools import reduce
from typing import Any, Dict, List, Tuple, Union

import jsonschema
import numpy as np
import scipy.stats as stats

from ._types import ArrayLike
from .config import ParamValidationError, fill_parameter
from .run import SweepRun


def q_log_uniform_v1_ppf(x: ArrayLike, min, max, q):
    r = np.exp(stats.uniform.ppf(x, min, max - min))
    ret_val = np.round(r / q) * q
    if isinstance(q, int):
        return ret_val.astype(int)
    else:
        return ret_val


def inv_log_uniform_v1_ppf(x: ArrayLike, min, max):
    return np.exp(
        -stats.uniform.ppf(
            1 - x,
            min,
            max - min,
        )
    )


def loguniform_v1_ppf(x: ArrayLike, min, max):
    return np.exp(stats.uniform.ppf(x, min, max - min))


class HyperParameter:

    CONSTANT = "param_single_value"
    CATEGORICAL = "param_categorical"
    CATEGORICAL_PROB = "param_categorical_w_probabilities"
    INT_UNIFORM = "param_int_uniform"
    UNIFORM = "param_uniform"

    LOG_UNIFORM_V1 = "param_loguniform"
    LOG_UNIFORM_V2 = "param_loguniform_v2"

    INV_LOG_UNIFORM_V1 = "param_inv_loguniform"
    INV_LOG_UNIFORM_V2 = "param_inv_loguniform_v2"

    Q_UNIFORM = "param_quniform"

    Q_LOG_UNIFORM_V1 = "param_qloguniform"
    Q_LOG_UNIFORM_V2 = "param_qloguniform_v2"

    NORMAL = "param_normal"
    Q_NORMAL = "param_qnormal"
    LOG_NORMAL = "param_lognormal"
    Q_LOG_NORMAL = "param_qlognormal"
    BETA = "param_beta"
    Q_BETA = "param_qbeta"

    def __init__(self, name: str, config: dict):
        """A hyperparameter to optimize.

        >>> parameter = HyperParameter('int_unif_distributed', {'min': 1, 'max': 10})
        >>> assert parameter.config['min'] == 1
        >>> parameter = HyperParameter('normally_distributed', {'distribution': 'normal'})
        >>> assert np.isclose(parameter.config['mu'], 0)

        Args:
            name: The name of the hyperparameter.
            config: Hyperparameter config dict.
        """

        self.name = name

        result = fill_parameter(name, config)
        if result is None:
            raise jsonschema.ValidationError(
                f"invalid hyperparameter configuration: {name}"
            )

        self.type, self.config = result

        if self.config is None or self.type is None:
            raise ValueError(
                "list of allowed schemas has length zero; please provide some valid schemas"
            )

        self.value = (
            None if self.type != HyperParameter.CONSTANT else self.config["value"]
        )

    def value_to_idx(self, value: Any) -> int:
        """Get the index of the value of a categorically distributed HyperParameter.

        >>> parameter = HyperParameter('a', {'values': [1, 2, 3]})
        >>> assert parameter.value_to_idx(2) == 1

        Args:
             value: The value to look up.

        Returns:
            The index of the value.
        """

        if self.type != HyperParameter.CATEGORICAL:
            raise ValueError("Can only call value_to_idx on categorical variable")

        for ii, test_value in enumerate(self.config["values"]):
            if value == test_value:
                return ii

        raise ValueError(
            f"{value} is not a permitted value of the categorical hyperparameter {self.name} "
            f"in the current sweep."
        )

    def cdf(self, x: ArrayLike) -> ArrayLike:
        """Cumulative distribution function (CDF).

        In probability theory and statistics, the cumulative distribution function
        (CDF) of a real-valued random variable X, is the probability that X will
        take a value less than or equal to x.

        Args:
             x: Parameter values to calculate the CDF for. Can be scalar or 1-d.
        Returns:
            Probability that a random sample of this hyperparameter will be less
            than or equal to x.
        """
        if self.type == HyperParameter.CONSTANT:
            return np.zeros_like(x)
        elif self.type == HyperParameter.CATEGORICAL:
            # NOTE: Indices expected for categorical parameters, not values.
            return stats.randint.cdf(x, 0, len(self.config["values"]))
        elif self.type == HyperParameter.CATEGORICAL_PROB:
            return np.cumsum(self.config["probabilities"])[x]
        elif self.type == HyperParameter.INT_UNIFORM:
            return stats.randint.cdf(x, self.config["min"], self.config["max"] + 1)
        elif (
            self.type == HyperParameter.UNIFORM or self.type == HyperParameter.Q_UNIFORM
        ):
            return stats.uniform.cdf(
                x, self.config["min"], self.config["max"] - self.config["min"]
            )
        elif (
            self.type == HyperParameter.LOG_UNIFORM_V1
            or self.type == HyperParameter.Q_LOG_UNIFORM_V1
        ):
            return stats.uniform.cdf(
                np.log(x), self.config["min"], self.config["max"] - self.config["min"]
            )
        elif (
            self.type == HyperParameter.LOG_UNIFORM_V2
            or self.type == HyperParameter.Q_LOG_UNIFORM_V2
        ):
            return stats.uniform.cdf(
                np.log(x),
                np.log(self.config["min"]),
                np.log(self.config["max"]) - np.log(self.config["min"]),
            )
        elif self.type == HyperParameter.INV_LOG_UNIFORM_V1:
            return 1 - stats.uniform.cdf(
                np.log(1 / x),
                self.config["min"],
                self.config["max"] - self.config["min"],
            )
        elif self.type == HyperParameter.INV_LOG_UNIFORM_V2:
            return 1 - stats.uniform.cdf(
                np.log(1 / x),
                -np.log(self.config["max"]),
                np.abs(np.log(self.config["max"]) - np.log(self.config["min"])),
            )
        elif self.type == HyperParameter.NORMAL or self.type == HyperParameter.Q_NORMAL:
            return stats.norm.cdf(x, loc=self.config["mu"], scale=self.config["sigma"])
        elif (
            self.type == HyperParameter.LOG_NORMAL
            or self.type == HyperParameter.Q_LOG_NORMAL
        ):
            return stats.lognorm.cdf(
                x, s=self.config["sigma"], scale=np.exp(self.config["mu"])
            )
        elif self.type == HyperParameter.BETA or self.type == HyperParameter.Q_BETA:
            return stats.beta.cdf(x, a=self.config["a"], b=self.config["b"])
        else:
            raise ValueError("Unsupported hyperparameter distribution type")

    def ppf(self, x: ArrayLike) -> Any:
        """Percentage point function (PPF).

        In probability theory and statistics, the percentage point function is
        the inverse of the CDF: it returns the value of a random variable at the
        xth percentile.

        Args:
             x: Percentiles of the random variable. Can be scalar or 1-d.
        Returns:
            Value of the random variable at the specified percentile.
        """
        if np.any((x < 0.0) | (x > 1.0)):
            raise ValueError("Can't call ppf on value outside of [0,1]")
        elif self.type == HyperParameter.CONSTANT:
            return self.config["value"]
        elif self.type == HyperParameter.CATEGORICAL:
            # Samples uniformly over the values
            retval = [
                self.config["values"][i]
                for i in np.atleast_1d(
                    stats.randint.ppf(x, 0, len(self.config["values"])).astype(int)
                ).tolist()
            ]
            if np.isscalar(x):
                return retval[0]
            return retval
        elif self.type == HyperParameter.CATEGORICAL_PROB:
            # Samples by specified categorical distribution if specified
            cdf = np.cumsum(self.config["probabilities"])
            if np.isscalar(x):
                return self.config["values"][np.argmin(x >= cdf, axis=-1)]
            else:
                return [
                    self.config["values"][i] for i in [np.argmin(cdf >= p) for p in x]
                ]
        elif self.type == HyperParameter.INT_UNIFORM:
            return (
                stats.randint.ppf(x, self.config["min"], self.config["max"] + 1)
                .astype(int)
                .tolist()
            )
        elif self.type == HyperParameter.UNIFORM:
            return stats.uniform.ppf(
                x, self.config["min"], self.config["max"] - self.config["min"]
            )
        elif self.type == HyperParameter.Q_UNIFORM:
            r = stats.uniform.ppf(
                x, self.config["min"], self.config["max"] - self.config["min"]
            )
            ret_val = np.round(r / self.config["q"]) * self.config["q"]
            if isinstance(self.config["q"], int):
                return ret_val.astype(int)
            else:
                return ret_val
        elif self.type == HyperParameter.LOG_UNIFORM_V1:
            return loguniform_v1_ppf(x, self.config["min"], self.config["max"])
        elif self.type == HyperParameter.LOG_UNIFORM_V2:
            return loguniform_v1_ppf(
                x, np.log(self.config["min"]), np.log(self.config["max"])
            )
        elif self.type == HyperParameter.INV_LOG_UNIFORM_V1:
            return inv_log_uniform_v1_ppf(x, self.config["min"], self.config["max"])
        elif self.type == HyperParameter.INV_LOG_UNIFORM_V2:
            return inv_log_uniform_v1_ppf(
                x, -np.log(self.config["max"]), -np.log(self.config["min"])
            )
        elif self.type == HyperParameter.Q_LOG_UNIFORM_V1:
            return q_log_uniform_v1_ppf(
                x, self.config["min"], self.config["max"], self.config["q"]
            )
        elif self.type == HyperParameter.Q_LOG_UNIFORM_V2:
            return q_log_uniform_v1_ppf(
                x,
                np.log(self.config["min"]),
                np.log(self.config["max"]),
                self.config["q"],
            )
        elif self.type == HyperParameter.NORMAL:
            return stats.norm.ppf(x, loc=self.config["mu"], scale=self.config["sigma"])
        elif self.type == HyperParameter.Q_NORMAL:
            r = stats.norm.ppf(x, loc=self.config["mu"], scale=self.config["sigma"])
            ret_val = np.round(r / self.config["q"]) * self.config["q"]
            if isinstance(self.config["q"], int):
                return ret_val.astype(int)
            else:
                return ret_val
        elif self.type == HyperParameter.LOG_NORMAL:
            # https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.lognorm.html
            return stats.lognorm.ppf(
                x, s=self.config["sigma"], scale=np.exp(self.config["mu"])
            )
        elif self.type == HyperParameter.Q_LOG_NORMAL:
            r = stats.lognorm.ppf(
                x, s=self.config["sigma"], scale=np.exp(self.config["mu"])
            )
            ret_val = np.round(r / self.config["q"]) * self.config["q"]

            if isinstance(self.config["q"], int):
                return ret_val.astype(int)
            else:
                return ret_val

        elif self.type == HyperParameter.BETA:
            return stats.beta.ppf(x, a=self.config["a"], b=self.config["b"])
        elif self.type == HyperParameter.Q_BETA:
            r = stats.beta.ppf(x, a=self.config["a"], b=self.config["b"])
            ret_val = np.round(r / self.config["q"]) * self.config["q"]
            if isinstance(self.config["q"], int):
                return ret_val.astype(int)
            else:
                return ret_val
        else:
            raise ValueError("Unsupported hyperparameter distribution type")

    def sample(self) -> Any:
        """Randomly sample a value from the distribution of this HyperParameter."""
        return self.ppf(random.uniform(0.0, 1.0))

```