# Node Software
<i class="fa fa-github-square fa-lg"></i> [Link to the project repo](https://github.com/slevasse/node-mki-firmware)

* The node software is written in C++.
* The node is controled by a simple state machine (see bellow).
* In the current state of the system, there no real need for a state machine since all operations are pretty much sequential with minimum branching.


## MK1 State machine
 ![](img/state_machine.png){style="width:70%"}
  <p style="text-align: center;">**Figure 1:** Simple node state machine.</p>

### States description
#### Init (state 0):
The state in which we initialise the system after powering-up.

* Read the config file in the external memory (SD card)
* Configure the system based on the config file
	* Set the wireless connection variable
	* If the file/sd-card is absent trigger a config-file error and go to the error state. 

#### Sleep (state 1):
The state in which we sleep until it is time to measure.

#### Measure (state 2):
The state in which we perfom the measurements.

#### Transmit (state 3):
The state in which we transmit the measured data to the server.

#### Save (state 4):
The state in which we save the measured data to the external memory.

#### Error (state 5):
The default state where we handle all systen breaking error detected by the system.
For now i am not sure if I need this bbut well better safe than sorry...