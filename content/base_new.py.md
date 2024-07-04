
The main purpose of the file `base_new.py` is to define a state machine and its transitions. 

The state machine is used to model the behaviour of a trader in a trading simulation. 

The transitions in the state machine represent the different states and actions that the trader can be in, and the conditions and actions that occur when moving between those states. 

See more here: [[Trader States]]

The file also defines several functions that are used in the transitions, such as `compute_final_spread (base_new.py:1280:5-1301:107)`, `volume_traded`, and `quanto_loss_w_avg`.