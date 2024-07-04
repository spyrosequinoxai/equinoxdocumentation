
### Summary

This report presents a detailed analysis of the best fit distributions for a series of different data variables collected from our operations. The primary objective is to demonstrate that our variables are not uniformly distributed but rather follow specific statistical distributions. This insight can be leveraged to improve the efficiency and effectiveness of Bayesian hyperparameter search.

---

### Analysis and Findings

#### 1. Variable: `current_r`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution captures the skewness and range of the data effectively, suggesting `current_r` is best modelled by this distribution.

![current_r](assets/plots/current_r.png)
#### 2. Variable: `entry_delta_spread`
- **Best Fit Distribution**: Student's t
- **Insights**: The Student's t distribution fits the data well, capturing the peak and tail behaviour effectively. This suggests that `entry_delta_spread` has heavier tails compared to a normal distribution.

![entry_delta_spread](assets/plots/entry_delta_spread.png)

#### 3. Variable: `entry_delta_spread2`
- **Best Fit Distribution**: Lognormal
- **Insights**: The lognormal distribution captures the skewness and long tail, indicating that `entry_delta_spread2` is likely log-normally distributed.

![entry_delta_spread2](assets/plots/entry_delta_spread2.png)

#### 4. Variable: `exit_delta_spread`
- **Best Fit Distribution**: Student's t
- **Insights**: The t distribution again fits the data well, reinforcing the heavy-tailed nature of `exit_delta_spread`.

![exit_delta_spread](assets/plots/exit_delta_spread.png)

#### 5. Variable: `exit_delta_spread2`
- **Best Fit Distribution**: Student's t
- **Insights**: The t distribution highlights the heavy tails and the peak in the data, similar to `exit_delta_spread`.

![exit_delta_spread2](exit_delta_spread2.png)

#### 6. Variable: `fastWeightSpot0`
- **Best Fit Distribution**: Weibull Max
- **Insights**: The Weibull max distribution captures the distribution well, with a clear peak and appropriate tail behavior.

![fastWeightSpot0](assets/plots/fastWeightSpot0.png)

#### 7. Variable: `fastWeightSpot1`
- **Best Fit Distribution**: Weibull Min
- **Insights**: The Weibull min distribution fits well, indicating that `fastWeightSpot1` is best modeled by this distribution.

![fastWeightSpot1](assets/plots/fastWeightSpot1.png)

#### 8. Variable: `fastWeightSpot2`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution captures the shape and range effectively, suggesting `fastWeightSpot2` follows a beta distribution.

![fastWeightSpot2](fastWeightSpot2.png)

#### 9. Variable: `fastWeightSwap0`
- **Best Fit Distribution**: Weibull Max
- **Insights**: The Weibull max distribution is the best fit, capturing the range and skewness of the data.

![fastWeightSwap0](assets/plots/fastWeightSwap0.png)

#### 10. Variable: `fastWeightSwap1`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data well, highlighting its suitability for `fastWeightSwap1`.

![fastWeightSwap1](assets/plots/fastWeightSwap1.png)

#### 11. Variable: `fastWeightSwap2`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution captures the skewness and range effectively, suggesting `fastWeightSwap2` follows a beta distribution.

![fastWeightSwap2](assets/plots/fastWeightSwap2.png)

#### 12. Variable: `funding_window`
- **Best Fit Distribution**: Pareto
- **Insights**: The Pareto distribution fits well, particularly capturing the heavy tail, indicating its suitability for modeling extreme values.

![funding_window](funding_window.png)

#### 13. Variable: `num_of_points_to_lookback_entry`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data well, capturing the range and skewness effectively.

![num_of_points_to_lookback_entry](num_of_points_to_lookback_entry.png)

#### 14. Variable: `quanto_threshold`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution captures the shape and range well, indicating it is the best fit for `quanto_threshold`.

![quanto_threshold](assets/plots/quanto_threshold.png)

#### 15. Variable: `ratio_entry_band_mov_ind`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data effectively, capturing the skewness and range.

![ratio_entry_band_mov_ind](ratio_entry_band_mov_ind.png)

#### 16. Variable: `rolling_time_window_size`
- **Best Fit Distribution**: Lognormal
- **Insights**: The lognormal distribution captures the skewness and long tail, indicating that `rolling_time_window_size` is likely log-normally distributed.

![rolling_time_window_size](rolling_time_window_size.png)

#### 17. Variable: `slow_funding_window`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits well, particularly capturing the skewness and range.

![slow_funding_window](slow_funding_window.png)

#### 18. Variable: `slowWeightSpot0`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution captures the shape and range effectively, suggesting `slowWeightSpot0` follows a beta distribution.

![slowWeightSpot0](assets/plots/slowWeightSpot0.png)

#### 19. Variable: `slowWeightSpot1`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data well, highlighting its suitability for `slowWeightSpot1`.

![slowWeightSpot1](assets/plots/slowWeightSpot1.png)

#### 20. Variable: `slowWeightSwap0`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits well, particularly capturing the skewness and range. This suggests `slowWeightSwap0` follows a beta distribution.

![slowWeightSwap0](assets/plots/slowWeightSwap0.png)

#### 21. Variable: `slowWeightSwap1`
- **Best Fit Distribution**: Uniform
- **Insights**: The uniform distribution fits the data, capturing the range effectively. This suggests `slowWeightSwap1` follows a uniform distribution, which is a notable deviation from the other variables.

![slowWeightSwap1](assets/plots/slowWeightSwap1.png)

#### 22. Variable: `weightSpot0`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data well, highlighting its suitability for `weightSpot0`.

![weightSpot0](assets/plots/weightSpot0.png)

#### 23. Variable: `weightSpot1`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution captures the shape and range effectively, suggesting `weightSpot1` follows a beta distribution.

![weightSpot1](assets/plots/weightSpot1.png)

#### 24. Variable: `weightSwap0`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data well, highlighting its suitability for `weightSwap0`.

![weightSwap0](assets/plots/weightSwap0.png)

#### 25. Variable: `weightSwap1`
- **Best Fit Distribution**: Beta
- **Insights**: The beta distribution fits the data well, highlighting its suitability for `weightSwap1`.

![weightSwap1](assets/plots/weightSwap1.png)

#### 26. Variable: `window_size`
- **Best Fit Distribution**: Lognormal
- **Insights**: The lognormal distribution captures the skewness and long tail, indicating that `window_size` is likely log-normally distributed.

![window_size](assets/plots/window_size.png)


---

## The Sad Truth...

However since we are bound to the WANDB platform these distributions are not supported.
Here is the current implementation: [[Wandb Sweeps Code]]
The supported distributions are:
*   CONSTANT
*   CATEGORICAL 
*   CATEGORICAL_PROB 
*   INT_UNIFORM 
*   UNIFORM with subcategories
	*  LOG_UNIFORM
	*  INV_LOG_UNIFORM
*  NORMAL with subcategories
	* LOG_NORMAL
* BETA


Furthermore all the distributions can also be found in a Quantized Version.

---
## Re-Evaluation

Let us go over the parameters again, this time finding the best out of the available distributions:

### 1. Variable: `quanto_threshold`
- **Best Fit Distribution**: Beta
![quanto_threshold](assets/plots2/quanto_threshold.png)

### 2. Variable: `current_r`
- **Best Fit Distribution**: Beta
![current_r](assets/plots2/current_r.png)

### 3. Variable: `entry_delta_spread`
- **Best Fit Distribution**: Beta
![entry_delta_spread](assets/plots2/entry_delta_spread.png)

### 4. Variable: `entry_delta_spread2`
- **Best Fit Distribution**: Beta
![entry_delta_spread2](assets/plots2/entry_delta_spread2.png)

### 5. Variable: `exit_delta_spread`
- **Best Fit Distribution**: Beta
![exit_delta_spread](assets/plots2/exit_delta_spread.png)

### 6. Variable: `fastWeightSpot0`
- **Best Fit Distribution**: Beta
![fastWeightSpot0](assets/plots2/fastWeightSpot0.png)

### 7. Variable: `fastWeightSpot1`
- **Best Fit Distribution**: Beta
![fastWeightSpot1](assets/plots2/fastWeightSpot1.png)

### 8. Variable: `fastWeightSwap0`
- **Best Fit Distribution**: Beta
![fastWeightSwap0](assets/plots2/fastWeightSwap0.png)

### 9. Variable: `fastWeightSwap1`
- **Best Fit Distribution**: Beta
![fastWeightSwap1](assets/plots2/fastWeightSwap1.png)

### 10. Variable: `fastWeightSwap2`
- **Best Fit Distribution**: Beta
![fastWeightSwap2](assets/plots2/fastWeightSwap2.png)

### 11. Variable: `weightSwap0`
- **Best Fit Distribution**: Beta
![weightSwap0](assets/plots2/weightSwap0.png)

### 12. Variable: `slowWeightSpot0`
- **Best Fit Distribution**: Beta
![slowWeightSpot0](assets/plots2/slowWeightSpot0.png)

### 13. Variable: `slowWeightSpot1`
- **Best Fit Distribution**: Beta
![slowWeightSpot1](assets/plots2/slowWeightSpot1.png)

### 14. Variable: `slowWeightSwap0`
- **Best Fit Distribution**: Beta
![slowWeightSwap0](assets/plots2/slowWeightSwap0.png)

### 15. Variable: `slowWeightSwap1`
- **Best Fit Distribution**: Beta
![slowWeightSwap1](assets/plots2/slowWeightSwap1.png)

### 16. Variable: `slowWeightSwap2`
- **Best Fit Distribution**: Beta
![slowWeightSwap2](assets/plots2/slowWeightSwap2.png)

### 17. Variable: `weightSpot0`
- **Best Fit Distribution**: Beta
![weightSpot0](assets/plots2/weightSpot0.png)

### 18. Variable: `weightSpot1`
- **Best Fit Distribution**: Beta
![weightSpot1](assets/plots2/weightSpot1.png)

### 19. Variable: `weightSpot2`
- **Best Fit Distribution**: Beta
![weightSpot2](assets/plots2/weightSpot2.png)


---
Takeaway: 
With proper scaling, we are kind of lucky because most of our parameters can be easily approximated by a beta distribution or a uniform one.



---
# Extra Optimization: Quantization
