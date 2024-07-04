
```
def spread_entry_func_numba(entry_swap, entry_spot, swap_fee, spot_fee):  
    return entry_swap * (1 - swap_fee) - entry_spot * (1 + spot_fee)  
  
def spread_exit_func_numba(exit_swap, exit_spot, swap_fee, spot_fee):  
    return exit_swap * (1 + swap_fee) - exit_spot * (1 - spot_fee)  
  
def spread_entry_func_bp_numba(entry_swap, entry_spot, swap_fee, spot_fee):  
    return (entry_swap * (1 - swap_fee) - entry_spot * (1 + spot_fee)) / entry_swap * 10000  
  
def spread_exit_func_bp_numba(exit_swap, exit_spot, swap_fee, spot_fee):  
    return (exit_swap * (1 + swap_fee) - exit_spot * (1 - spot_fee)) / exit_swap * 10000  
```

entry_swap: The swap of a Bid / Buy
entry_spot: The spot of an Ask / Sell

BP probably converts everything to Base Points.
