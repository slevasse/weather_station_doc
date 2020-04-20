# Finite state machine implementation

## Sources
* I have pretty much stolen the structure of the FSM from [this](https://aleksandrhovhannisyan.github.io/blog/dev/finite-state-machine-fsm-tutorial-implementing-an-fsm-in-c/) example.
Things were well explained and I could apply it to my project quickly.

## Implementation

!!! warning
    This is in no way a tutorial or something intended for people to reproduce this project.
    It iss only here for me to remember how I did things.

### Disclaimer
I could have done it in a much simpler way since, so far, all opperation are pretty much sequential.
Thus I do not need a finite state machine to run the system.
However, I will use this code as a base to expend on the system and it is quite sure that the complexity of things will increase.
Moreover, I have never implemented a FSM in OOP style before and I thought this was a good opportunity.
(This is why I implemented this OOP state machine).

### Overview
The FSM is a class which instanciate a new state object at each state transition.
Each state object inherit from a generic state obbject. 
There are five files in total.

### GenericState class
* This class is a generic state used to create each state object.
* Contained in a single file [GenericState.h](https://github.com/slevasse/node-mki-firmware/blob/master/main/GenericState.h)

The code:
```CPP
#pragma once
#include "FiniteStateMachine.h"
// Abstract state class

// Forward declaration to resolve circular dependency/include
class FiniteStateMachine;

class GenericState
{
public:
  virtual void enter(FiniteStateMachine* fsm) = 0;
  virtual void toggle(FiniteStateMachine* fsm) = 0;
  virtual void exit(FiniteStateMachine* fsm) = 0;
  virtual ~GenericState() {}
};
```

* The `enter` method is here to do things when we enter the state.
	* This is pretty much where all the work will go.
* The `toggle` method is here to change state.
	* it is called once we are done working in this state.
* The `exit` method is here to do things after we changed state.
	* I don't see a use for it now.. Perhaps some cleaning when needed.

### SystemStates classes
* These files [SystemStates.h](https://github.com/slevasse/node-mki-firmware/blob/master/main/SystemStates.h) / [SystemStates.cpp](https://github.com/slevasse/node-mki-firmware/blob/master/main/SystemStates.cpp) contain the implementation of each state.
* Depending how busy each state will become, I might move them to their own files.

SystemStates.h:
```CPP
#pragma once
#include "GenericState.h"
#include "FiniteStateMachine.h"
#include <Arduino.h>
#include <HardwareSerial.h>

class Initialise : public GenericState
{
public:
  void enter(FiniteStateMachine* fsm);
  void toggle(FiniteStateMachine* fsm);
  void exit(FiniteStateMachine* fsm) {}
  static GenericState& getInstance();
  int stateNumber = 0;

private:
  Initialise() {}
  Initialise(const Initialise& other);
  Initialise& operator=(const Initialise& other);
};


class Sleep : public GenericState
{
public:
  void enter(FiniteStateMachine* fsm);
  void toggle(FiniteStateMachine* fsm);
  void exit(FiniteStateMachine* fsm) {}
  static GenericState& getInstance();
  int stateNumber = 1;

private:
  Sleep() {}
  Sleep(const Sleep& other);
  Sleep& operator=(const Sleep& other);
};


class Measure : public GenericState
{
public:
  void enter(FiniteStateMachine* fsm);
  void toggle(FiniteStateMachine* fsm);
  void exit(FiniteStateMachine* fsm) {}
  static GenericState& getInstance();
  int stateNumber = 2;

private:
  Measure() {}
  Measure(const Measure& other);
  Measure& operator=(const Measure& other);
};


class Transmit : public GenericState
{
public:
  void enter(FiniteStateMachine* fsm);
  void toggle(FiniteStateMachine* fsm);
  void exit(FiniteStateMachine* fsm) {}
  static GenericState& getInstance();
  int stateNumber = 3;

private:
  Transmit() {}
  Transmit(const Transmit& other);
  Transmit& operator=(const Transmit& other);
};

class Save : public GenericState
{
public:
  void enter(FiniteStateMachine* fsm);
  void toggle(FiniteStateMachine* fsm);
  void exit(FiniteStateMachine* fsm) {}
  static GenericState& getInstance();
  int stateNumber = 4;

private:
  Save() {}
  Save(const Save& other);
  Save& operator=(const Save& other);
};

class Error : public GenericState
{
public:
  void enter(FiniteStateMachine* fsm);
  void toggle(FiniteStateMachine* fsm);
  void exit(FiniteStateMachine* fsm) {}
  static GenericState& getInstance();
  int stateNumber = 5;

private:
  Error() {}
  Error(const Error& other);
  Error& operator=(const Error& other);
};
```

* I do not know why there is so many constructors (3/class). But I do not care, it works.

SystemStates.cpp:
```CPP
#include "SystemStates.h"

//==============================================================================
// Initialise State
//==============================================================================
void Initialise::toggle(FiniteStateMachine* fsm)
{
  // change state from init to sleep
  // for now I'll ignore the error state
  // use the fsm object we passed to create an instance of the next state.
  fsm->setState(Sleep::getInstance());
}


GenericState& Initialise::getInstance()
{
  static Initialise singleton;
  return singleton;
}

void Initialise::enter(FiniteStateMachine* fsm)
{
  Serial.println("entered Init state");
  delay(2000);
}

//==============================================================================
// Sleep State
//==============================================================================
void Sleep::toggle(FiniteStateMachine* fsm)
{
  // change state from sleep to measure
  // for now I'll ignore the error state
  // use the fsm object we passed to create an instance of the next state.
  fsm->setState(Measure::getInstance());
}


GenericState& Sleep::getInstance()
{
  static Sleep singleton;
  return singleton;
}

void Sleep::enter(FiniteStateMachine* fsm)
{
  Serial.println("entered Sleep state");
  delay(2000);
  //LowPower.sleep(4000); DOES NOT WORK
}

//==============================================================================
// Measure State
//==============================================================================
void Measure::toggle(FiniteStateMachine* fsm)
{
  // change state from measure to transmit
  // for now I'll ignore the error state
  // use the fsm object we passed to create an instance of the next state.
  fsm->setState(Transmit::getInstance());
}


GenericState& Measure::getInstance()
{
  static Measure singleton;
  return singleton;
}

void Measure::enter(FiniteStateMachine* fsm)
{
  Serial.println("entered measure state");
  delay(500);
}


//==============================================================================
// Transmit State
//==============================================================================
void Transmit::toggle(FiniteStateMachine* fsm)
{
  // change state from transmit to save
  // for now I'll ignore the error state
  // use the fsm object we passed to create an instance of the next state.
  fsm->setState(Save::getInstance());
}


GenericState& Transmit::getInstance()
{
  static Transmit singleton;
  return singleton;
}

void Transmit::enter(FiniteStateMachine* fsm)
{
  Serial.println("entered transmit state");
  delay(500);
}

//==============================================================================
// Save State
//==============================================================================
void Save::toggle(FiniteStateMachine* fsm)
{
  // change state from Save to sleep
  // for now I'll ignore the error state
  // use the fsm object we passed to create an instance of the next state.
  fsm->setState(Sleep::getInstance());
}


GenericState& Save::getInstance()
{
  static Save singleton;
  return singleton;
}

void Save::enter(FiniteStateMachine* fsm)
{
  Serial.println("entered Save state");
  delay(500);
}

//==============================================================================
// Error State
//==============================================================================
void Error::toggle(FiniteStateMachine* fsm)
{
  // change state from Save to sleep
  // for now I'll ignore the error state
  // use the fsm object we passed to create an instance of the next state.
  fsm->setState(Initialise::getInstance());
}


GenericState& Error::getInstance()
{
  static Error singleton;
  return singleton;
}

void Error::enter(FiniteStateMachine* fsm)
{
  Serial.println("entered error state");
  delay(500);
}

```

* This is just the squeleton, none of the actual work is perfomed in these class at the moment.
* The `toggle` method changes the state of the FinitStateMachine object it was instanciated into to the next state. It does so by passing a singleton of the next state class.
	* There we can add more decision making later..
	* For now the transition is braindead.
* The `getInstance` method instanciate a singleton of the state class and rerurns it.
* The `enter` method is filled with debug code. 

### FiniteStateMachine class
* This class is the FSM and it glues all the state class together.
* It has two files [FiniteStateMachine.cpp](https://github.com/slevasse/node-mki-firmware/blob/master/main/FiniteStateMachine.cpp) and [FiniteStateMachine.h](https://github.com/slevasse/node-mki-firmware/blob/master/main/FiniteStateMachine.h).

FiniteStateMachine.h:
```CPP
#pragma once
#include "GenericState.h"

class GenericState;

class FiniteStateMachine
{
public:
  // Constructor, init the state machine
  FiniteStateMachine();  
  // toggle the state machine
  void toggle();
  // inline function to get the current state of the FSM
  inline GenericState* getCurrentState() const { return currentState; }
  // Set the FSM state
  void setState(GenericState& newState);

private:
  // the fsm state
  GenericState* currentState;
};
```

FiniteStateMachine.cpp:

```CPP
#include "FiniteStateMachine.h"
#include "SystemStates.h"

FiniteStateMachine::FiniteStateMachine()
{
  // All the system starts in the init state
  currentState = &Initialise::getInstance();
}

void FiniteStateMachine::setState(GenericState& newState)
{
  currentState->exit(this);  // do stuff before we change state
  currentState = &newState;
  currentState->enter(this); // do stuff after we change state
}

// ask the current state to change state
void FiniteStateMachine::toggle()
{
  // Delegate the task of determining the next state to the current state
  currentState->toggle(this); 
}

```

* The constructor set the current state to the `initialise` state.
* The `setState` method calls the `exit` method of the current state, changes the state to the next one (which is passed as an argument by the previous state) and calls the `enter` method of the state we just entered.
* The `toggle` method is here to manualy request for the next state, it is used for debug.
	* We will most probably not need it later as, I guess, the state will call its `toggle` method y itself when it is done working (at the end of its `enter` method).

### Test code
This is the code I used to test the good function of the FSM.
It just instanciates the FSM object, open a serial connection to my pc and toggle the states.

```CPP  
#include "FiniteStateMachine.h"

int delayValue = 2000;
FiniteStateMachine f

void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
  Serial.println("start test");
  delay(delayValue);

void loop() {
  // run the fsm forever
  fsm.toggle();
}
```
This yeild the folowing serial print.
```
14:13:30.527 -> entered Sleep state
14:13:32.504 -> entered measure state
14:13:33.035 -> entered transmit state
14:13:33.519 -> entered Save state
14:13:34.011 -> entered Sleep state
14:13:36.015 -> entered measure state
14:13:36.502 -> entered transmit state
14:13:37.022 -> entered Save state
14:13:37.514 -> entered Sleep state
```
* The "init state" and "start test" messages are not displayed since it takes some time for the serial monitor to start.
I am sure there is some way to fix that but I do not really care...