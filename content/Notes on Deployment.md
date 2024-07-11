### Simulation Setup

* Wandb staging equinox --> Create new Project
* Sweeps --> Copy YAML config
* Paste in new Sweep Config

--- 
##### FINDING DATES e.g. 21/4 - 10/6
* Convert to milliseconds the start / end period
* Overview --> Rename the sweep to the name symbol exchange\_(swap)\_from\_to
* Go to Workspace --> Paste the link to Monday
* Sweep --> Stop Manually


--------------------

### Confirmation
[https://docs.google.com/spreadsheets/d/1MDGVBsWbVq6KxOSl2nofL-I7F2lc_e80d3zX7gTE3JM/edit?gid=1655387721#gid=1655387721](https://docs.google.com/spreadsheets/d/1MDGVBsWbVq6KxOSl2nofL-I7F2lc_e80d3zX7gTE3JM/edit?gid=1655387721#gid=1655387721)

* Open a new project bottom
* Add a new row with appropriate date
* Make a new sweep with title conformation
* CUSTOM FILTER = 'SIMULATION_RESULTS'
* SYMBOL = SYMBOL
----------------------------------------------------
* FILE_NAME = You must make it.

Go to the project, activate the filters from Cristian (IN RUNS) --> DOWNLOAD CSV

This CSV must be uploaded to Sim Machines 1,2,3,4
at Path: [/home/equinoxai/simulation_params/symbol]

---------------
Go Back to the google document and put the csv file name.

Take the Python code and run it in the machine. This will setup a local controller.
Then run the start_sweep.py script

Note: Check its progress with: docker logs -ft [docker_sweep_name]

--------------------
In case it breaks go back to Wandb and delete the sweep. 
Start again from the beginning.

If using docker logs -ft you see the columns error mismatch --> Its a data error.
Go and run the helper python function.

trader_expected_executions//simulation codebase //local_data_test 

It generates a report which files are corrupted and downloads them from the backblaze:
Arguments:
* to_check = prices
* download = true
* remove_corrupted = true
