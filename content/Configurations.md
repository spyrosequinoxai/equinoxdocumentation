
### XBTUSD - [[BitMEX]] // XBTUSD - [[Deribit]]

method: bayes
metric:
  goal: maximize
  name: Funding in Total
parameters:
  adjust_pnl_automatically:
    distribution: constant
    value: true
  band_funding_system:
    distribution: constant
    value: funding_continuous_weight_concept
  band_funding_system2:
    distribution: constant
    value: funding_continuous_weight_concept
  entry_delta_spread:
    distribution: uniform
    max: 15
    min: 1.5
  entry_delta_spread2:
    distribution: uniform
    max: 15
    min: 1.5
  exchange_spot:
    distribution: constant
    value: Deribit
  exchange_swap:
    distribution: constant
    value: BitMEX
  exit_delta_spread:
    distribution: uniform
    max: 15
    min: 1.5
  exit_delta_spread2:
    distribution: uniform
    max: 15
    min: 1.5
  fastWeightSpot0:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSpot1:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSwap0:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSwap1:
    distribution: uniform
    max: 5
    min: 0
  filter_only_training:
    distribution: constant
    value: true
  funding_options:
    distribution: constant
    value: all
  funding_periods_lookback:
    distribution: int_uniform
    max: 30
    min: 1
  funding_window:
    distribution: int_uniform
    max: 1000
    min: 1
  hoursBeforeSwap0:
    distribution: constant
    value: 8
  hoursBeforeSwap1:
    distribution: constant
    value: 0
  max_trade_volume:
    distribution: categorical
    values:
      - 1000
      - 2000
      - 3000
      - 4000
      - 5000
      - 6000
      - 7000
      - 8000
      - 9000
      - 10000
  maximum_quality:
    distribution: constant
    value: 2
  slow_funding_window:
    distribution: int_uniform
    max: 100000
    min: 1000
  slowWeightSpot0:
    distribution: uniform
    max: 5
    min: 0
  slowWeightSpot1:
    distribution: uniform
    max: 5
    min: 0
  slowWeightSwap0:
    distribution: uniform
    max: 5
    min: 0
  slowWeightSwap1:
    distribution: uniform
    max: 5
    min: 0
  spot_fee:
    distribution: constant
    value: 0.0003
  spot_instrument:
    distribution: constant
    value: BTC-PERPETUAL
  swap_fee:
    distribution: constant
    value: -0.00011
  swap_instrument:
    distribution: constant
    value: XBTUSD
  t_end_period:
    distribution: constant
    value: 1719439200000~1694206800000~1711918800000~1716325200000~1719439200000
  t_start_period:
    distribution: constant
    value: 1713650400000~1689800400000~1696107600000~1707948000000~1717970400000
  train_multiple_periods:
    distribution: constant
    value: true
  use_same_slowSwap_slowSpot_generic_funding:
    distribution: constant
    value: false
  use_same_window_size:
    distribution: constant
    value: true
  window_size:
    distribution: int_uniform
    max: 4000
    min: 1000
program: src/scripts/wandb_sweeps/taker_maker_xbtusd_no_symmetrical.py



### ETHUSDT - [[BitMEX]]

method: bayes
metric:
  goal: maximize
  name: ROR Annualized
parameters:
  adjust_pnl_automatically:
    distribution: constant
    value: true
  band_funding_system:
    distribution: constant
    value: funding_continuous_weight_concept
  band_funding_system2:
    distribution: constant
    value: funding_continuous_weight_concept
  entry_delta_spread:
    distribution: uniform
    max: 5
    min: 0
  entry_delta_spread2:
    distribution: uniform
    max: 5
    min: 0
  exchange_spot:
    distribution: constant
    value: Deribit
  exchange_swap:
    distribution: constant
    value: BitMEX
  exit_delta_spread:
    distribution: uniform
    max: 5
    min: 0
  exit_delta_spread2:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSpot0:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSpot1:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSwap0:
    distribution: uniform
    max: 5
    min: 0
  fastWeightSwap1:
    distribution: uniform
    max: 5
    min: 0
  filter_only_training:
    distribution: constant
    value: true
  funding_options:
    distribution: constant
    value: all
  funding_periods_lookback:
    distribution: int_uniform
    max: 30
    min: 1
  funding_window:
    distribution: int_uniform
    max: 1000
    min: 1
  hoursBeforeSwap0:
    distribution: constant
    value: 8
  hoursBeforeSwap1:
    distribution: constant
    value: 0
  max_trade_volume:
    distribution: categorical
    values:
      - 1000
      - 2000
      - 3000
      - 4000
      - 5000
      - 6000
      - 7000
      - 8000
      - 9000
      - 10000
  maximum_quality:
    distribution: constant
    value: 2
  slow_funding_window:
    distribution: int_uniform
    max: 100000
    min: 1000
  slowWeightSpot0:
    distribution: uniform
    max: 5
    min: 0
  slowWeightSpot1:
    distribution: uniform
    max: 5
    min: 0
  slowWeightSwap0:
    distribution: uniform
    max: 5
    min: 0
  slowWeightSwap1:
    distribution: uniform
    max: 5
    min: 0
  spot_fee:
    distribution: constant
    value: 0.0003
  spot_instrument:
    distribution: constant
    value: ETH-PERPETUAL
  swap_fee:
    distribution: constant
    value: -0.00011
  swap_instrument:
    distribution: constant
    value: ETHUSDT
  t_end_period:
    distribution: constant
    value: 1719180000000~1694206800000~1698789600000~1711918800000~1716325200000
  t_start_period:
    distribution: constant
    value: 1717797600000~1689800400000~1696107600000~1706738400000~1707948000000
  train_multiple_periods:
    distribution: constant
    value: true
  use_same_slowSwap_slowSpot_generic_funding:
    distribution: constant
    value: false
  use_same_window_size:
    distribution: constant
    value: true
  window_size:
    distribution: int_uniform
    max: 10000
    min: 500
program: src/scripts/wandb_sweeps/taker_maker_xbtusd_no_symmetrical.py

