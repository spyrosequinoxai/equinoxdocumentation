The states of the trading system are as follows:

#### States

1. **`clear`**
   - **On Enter**: `band_funding_adjustment`
   - **Description**: Initial state. Adjusts the funding bands when entering this state.

2. **`trying_to_post`**
   - **On Exit**: `swap_value`, `posting_f`, `trying_to_post_counter`, `keep_time_post`
   - **Description**: The system is attempting to post an order. Updates the swap value, increments the trying to post counter, and keeps track of the time spent in this state.

3. **`not_posted`**
   - **Description**: The system failed to post an order.

4. **`posted`**
   - **Description**: An order has been posted.

5. **`try_to_cancel`**
   - **Description**: The system is attempting to cancel a posted order.

6. **`executing`**
   - **On Enter**: `executing_f`
   - **Description**: The order is being executed. Handles the execution process when entering this state.

7. **`cancelled`**
   - **On Enter**: `cancelled_f`
   - **Description**: The order has been cancelled. Adds latency when entering the cancel state.

8. **`spot_balance`**
   - **On Enter**: `spot_balance_f`
   - **On Exit**: `swap_value`
   - **Description**: The system is in a state of balance after execution. Adds a spot balance latency when entering this state and updates the swap value on exit.


#### Conditions
1. **`try_post_condition`**
- **Purpose**: To check if the spread is available and if conditions are met to try posting.
- **Details**:
 - Checks if the spread is available.
 - If stop trading is enabled in `Quanto_loss`, it stops the trading for 8 hours.
 - Checks if the system is rate-limited.
 - Checks if the cumulative volume is above the max position allowed.
 - Checks if the area spread condition is met.

2. **`activation_function_try_post`**
- **Purpose**: To exit the `trying_to_post` state.
- **Details**:
 - Adds a latency to the values.
 - Checks if there is a price change in the swap prices within the time interval.
 - Returns the side of the execution that will be used later.

3. **`spread_unavailable_post`**
- **Purpose**: To check if the spread is unavailable or if the swap price moves in the wrong direction.
- **Details**:
 - Checks if the spread is unavailable or the swap price moved in the wrong direction.
 - In the entry side, the wrong direction is if the previous value is larger than the current value.
 - In the exit side, the wrong direction is if the previous value is smaller than the current value.

4. **`is_order_too_deep`**
- **Purpose**: To check if the order is too deep in the order book.
- **Details**:
 - Checks if the targeted depth  is less than the difference between the swap price and the entry/exit swap price.
 - Checks if the swap price is greater than the sum of the swap price and the predicted depth.

5. **`swap_price_movement_correct`**
- **Purpose**: To check if the known swap price moves towards the correct side.
- **Details**:
 - Checks if there is a movement of the known swap price towards the correct side (price increase on entry and price decrease on exit).

6. **`swap_price_no_movement`**
- **Purpose**: To check if there is no movement in the swap price.
- **Details**:
 - Returns `True` if there is no movement in the swap price by comparing the current known value with the previous one.

7. **`activation_function_cancel`**
- **Purpose**: To check if the conditions are met to cancel the order.
- **Details**:
 - Checks if the price swap moves against the trade in the time interval between now and now+latency.
 - In the entry side, checks if the previous swap price is greater than the current swap price and there is a bid volume.
 - In the exit side, checks if the previous swap price is less than the current swap price and there is an ask volume.

8. **`has_prices`**
- **Purpose**: To check if there are prices available.
- **Details**:
 - Finds the index of the known spot and swap prices.
 - Checks if there are prices available for both spot and swap.

9. **`not_has_prices`**
- **Purpose**: To check if there are no prices available.
- **Details**:
 - Returns the negation of the `has_prices` property.

#### Callbacks
1. **`quanto_loss_func`**
- **Purpose**: To compute the quanto profit or loss whenever conditions are met.
- **Details**:
 - Computes the quanto profit or loss if the previous computation happened some seconds in the past.
 - Updates the quanto system with the current timestamp and cumulative volume.
 - Adjusts the entry and exit bands with the quanto adjustments.

2. **`move_time_forward`**
- **Purpose**: To move time forward from one transition to another when no latency is included.
- **Details**:
 - Moves the timestamp to the next position in the dataframe.

3. **`update_time_latency_try_post`**
- **Purpose**: To update the timestamp with the latency while trying to post.
- **Details**:
 - Updates the timestamp and position in the dataframe with the latency for trying to post.

4. **`swap_value`**
- **Purpose**: To find the swap price on the exit of the `trying_to_post` state.
- **Details**:
 - Finds the swap price on the exit of the `trying_to_post` state to use it later to define the executing spread.

5. **`trying_to_post_counter`**
- **Purpose**: To keep a counter of the attempts to post.
- **Details**:
 - Appends the current timestamp to the list of trying to post counters.

6. **`posting_f`**
- **Purpose**: To handle the actions when entering the `posting` state.
- **Details**:
 - Updates the timestamp with the latency for trying to post.
 - Updates the position in the dataframe.

7. **`executing_f`**
- **Purpose**: To handle the actions when entering the `executing` state.
- **Details**:
 - Updates the timestamp with the latency for executing.
 - Updates the position in the dataframe.
 - Sets the source of the transition.

8. **`cancelled_f`**
- **Purpose**: To add latency when entering the `cancelled` state.
- **Details**:
 - Updates the timestamp with the latency for cancelling.
 - Updates the position in the dataframe.

9. **`spot_balance_f`**
- **Purpose**: To add a spot balance latency in order to enter the `spot_balance` state.
- **Details**:
 - Updates the timestamp with the latency for spot balance.
 - Updates the position in the dataframe.

10. **`compute_final_spread`**
- **Purpose**: To define the final spread.
- **Details**:
  - Computes the final spread using the swap price stored in the `trying_to_post` state and the real value of the spot price.

11. **`volume_traded`**
- **Purpose**: To compute the cumulative volume.
- **Details**:
  - Adds the traded volume if the side is entry.
  - Subtracts the traded volume if the side is exit.
  - Updates the total volume traded.

12. **`quanto_loss_w_avg`**
- **Purpose**: To compute the average weighted price for quanto profit-loss.
- **Details**:
  - Updates the trade in the quanto system with the current timestamp, cumulative volume, side, and traded volume.

13. **`band_funding_adjustment`**
- **Purpose**: To adjust the funding bands.
- **Details**:
  - Updates the funding system with the current timestamp if the timestamp exceeds the last update time plus the update interval.
  - Updates the entry and exit funding adjustments.
  - Updates the entry and exit bands with the funding adjustments.

14. **`keep_time_post`**
- **Purpose**: To keep the time post updated.
- **Details**:
  - Updates the time post if the transition destination is `posted`.
  - Updates the timestamp and position in the dataframe.