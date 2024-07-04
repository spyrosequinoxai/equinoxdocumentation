
## Spreads

Bid = Buy
Ask = Sell
Fee = A transaction cost you always pay to the exchange provider.

```
Spread Entry =  Swap(Bid) * (1 - Swap Fee) - Spot(Ask) * (1 + Spot Fee) 
Spread Exit  =  Swap(Ask) * (1 + Swap Fee) - Spot(Bid) * (1 - Spot Fee) 
```

See more at: [[spread_computation_function.py]]

-----
## Moving Average Metrics

***Useful Parameters (To be optimized in a Bayesian manner)***

window_size = lookback in minutes for the MAVG calculations.
entry_delta_spread = fixed value over the band as delta (positive value).
exit_delta_spread = fixed value under the band as delta (positive value).
(fast/slow)\_weight\_(swap/spot) = weights of importance in the following functions.

If we use as swap: [[Deribit]]:
W1 = Funding Window # 10 sec
W2 = Slow Funding Window # 100 sec

If we use as swap: [[BitMEX]]:
W1 = 1
W2 = Funding Periods Lookback


* ***Central Band*** = $\frac{MAVG(spread\space entry, window\space size) + MAVG(spread\space exit, window\space size)}{2}$
* ***Entry Band*** = Central Band + Entry delta spread + Extra entry band delta
* ***Exit Band***   = Central Band  + Exit delta spread   + Extra exit band delta


The extra entry and exit band deltas are calculated below:

$P_1 =Fast * MAVG(SwapFund,W1) + Slow*MAVG(SwapFund,W2)$
$P_2 = Fast * MAVG(SpotFund,W1) + Slow*MAVG(SpotFund,W2)$


Extra Entry Band Delta = $P_1 - P_2$
Extra Exit Band Delta = $P_2 - P_1$


---

The interesting case of "Between Funding Behaviour"

We hypothesize that between funding events we can linearly interpolate form 1 set of values to another set of values more appropriate to compensate for your lack of knowledge.

This reflects to a second set of parameters.


See more at: [[base_new.py]] 
