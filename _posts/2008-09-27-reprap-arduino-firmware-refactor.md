---
layout: post
title: "RepRap Arduino Firmware refactor"
date: 2008-09-27 22:55:37
categories: [Random, 3D Printing]
wordpress_id: 143
redirect_from: /?p=143
---

To teach the Karate kid how to defend himself, Mr. Miyagi used repetitive mundane tasks that mimic various defense maneuvers. This repetition created muscle memory for the moves which ultimately lead to victory during competition.



Patterns are the muscle memory of programmers. They are time tested generic solutions to generic pervasive problems. Patterns provide terminology, improve maintainability, are relatively self documenting, and improve reusability. While not appropriate in all situations and difficult to build in at design time, I  �� ��m very much a fan of refactoring to patterns.



I love Wiring. It  �� ��s a cute API which enables more people of diverse backgrounds to enjoy playing with Microcontrollers. Wiring as an API doesn  �� ��t enforce a design model on users of Microcontrollers, which I believe is both a strength and a weakness. As a strength, the lack of structure is more approachable to developers. As Wiring projects become larger and subject to more uses than hey originally built for, the lack of structure becomes a weakness - It becomes harder to share or reuse code.



Fabr uses a stepper motor for the extruder instead of an brushed motor. This has caused me problems integrating the RepRap code directly into my project. With the support of Zach, who created the original RepRap firmware for the Arduino, I  �� ��m going to attempt to refactor the code base to use patterns and implement device abstraction. 



## RepRap today


[![](/assets/images/2008/09/repraptoday.png)](/assets/images/2008/09/repraptoday.png)


The RepRap code is currently broken up into multiple functional groups:



- The Main Loop (GCode_interpreter.pde) - This component is responsible for initializing the other components, servicing the serial port, and driving the Gcode processor.


- The GCode Processor (process_string.pde) - This component is responsible for breaking command strings into constituent parts, and driving the Stepper manager or extruder as required by the command (such as setting temperature).


- The Stepper Manager (stepper_control.pde) - This component is the guts of the interface to the reprap. It manages the axes, and turns on and off the extruder motor.


- Extruder Manager (extruder.pde) - Manages the extruder motor, heater, and temperature feedback mechanism.







This design has a few limitations which I hope to address:



- There are several blocking loops (Main, Axes Stepper, Extruder)


- Each component has internal knowledge about the other components


- Tight coupling between the hardware implementation, the code to drive it


- The code assumes that hardware is local to the microcontroller







## Pattern Discussion


Here are several patterns which would be useful to microcontroller firmware.



### Event Loop Pattern


An event loop is a critical component of event driven programming. The loop waits for work (such as a mouse event), and delivers that work to the appropriate component (such as a window).



Steppers need to be stepped at a specific frequency in order to move. A trivial method of driving a stepper would be to sit in a tight loop stepping the motor. What if you need to drive multiple steppers? Traditionally the additional stepper control would be integrated into a single stepper loop, creating a tight coupling between the driving logic and the attached devices. 



To remedy this using the Event Loop Pattern, each stepper could register for a timed callback, which would pulse each stepper. There are several benefits to this:



- each stepper could be driven at different speeds (by setting different callback rates)


- driving code for a stepper is decoupled from the other steppers in the project


- because the code is decoupled, an individual stepper could be replaced with a different device







Additionally, the Event loop could be used to drive periodic events such as servicing a serial port, checking stop conditions, managing temperature, etc.



### Observable Pattern


When I first heard about this pattern, I thought   ��˜This has a name?  �� ��. An object which has state can be observed by one or more objects. When that state changes, the observers are notified and that state change can be acted upon. The state can change due to an interrupt firing, a polling event (during an event loop), or state being set by a another component.



There are several cases where this is interesting for microcontrollers - End stops, encoders, temperature changes, serial data available or command completion.  When the state changes (like an end stop triggering), the observers are notified (An axis object observing the stop now knows it is at the extent).  This can in turn fire new events (the axis object notifies the command interpreter that home has been reached).





### Interface Pattern


An interface is an abstraction which generalizes a class of functionality. In C++ this is done using an abstract base class. Typically interfaces implement some form of interface discovery mechanism (like Microsoft COM  �� ��s QueryInterface) and maybe memory management (AddRef/Release). This use of this pattern will become evident in future pattern discussions.



### Device Pattern


A device is something attached to the microcontroller. It can have a custom interface which more closely matches the reason for its existence, rather than how it exists. 



A stepper device could expose the generic motor interface which has properties like   ��˜rate  �� �� and   ��˜direction  �� ��, and methods like    ��˜stop  �� �� and   ��˜start  �� ��. This motor interface could be implemented using a stepper or brushed motor, or even a linear motion device. However, an Axis mechanism need only be written to the motor interface.



### Mechanism Pattern


A Mechanism is an opaque collection of devices with a custom interface for the composite mechanism. Consider a Cartesian mechanism - which is the component that the GCode interpreter is written to use. The Cartesian mechanism has an interface which includes properties like 'rate', and position properties like 'x', 'y', 'z', and maybe even spherical properties like 'zenith', 'azimuth', and 'radius'.  The cartesian mechanism could be built using 3 Axis mechanisms, or even a rotary table and longitudinal axis (although the naming breaks down; work with me here). Each axis could be implemented with different physical hardware, but exposed with the Axis interface to the Cartesian mechanism.



### Behavior Pattern


A Behavior is a decision making component which responds to external input and drives mechanisms or devices. An example of a behavior would be a GCode interpreter, which listens to a serial device, and executes commands on a Cartesian mechanism and Extruder Mechanism.



## Proposed Design


[![](/assets/images/2008/09/proposed.png)](/assets/images/2008/09/proposed.png)



### RepRap Design Discussion


#### At Startup:


During the initialization phase of the firmware, objects are created which represent each device, behavior and transport in the system. These objects are initialized with either persisted data, compiled data, or delay discover the bounds of the system. The initialized objects are then hooked into the appropriate components. Observations are made as objects are inserted into their mechanisms or applied to the behaviors.



#### Executing a Command:


The gcode behavior observes the serial transport. As the Event loop services the serial transport, there will come a time when an end of line is reached, and the serial port will notify its observers, then remove itself from the event loop until the line is flushed.



When the gcode behavior receives the line available event, it processes it, and acts appropriately:



If the command references the Extruder mechanism, the appropriate interface method is called; currently this means setting the desired temperature. In response to this interface call, the Extruder mechanism will register for periodic servicing from the event loop - This periodic event polls the temperature device, and adjusts the heater appropriately.



If the command is a positioning command, the gcode behavior will call the interface method on the Cartesian mechanism to instruct it to position itself to a specific point. The cartesian mechanism will then set the rate on each axis, and notify each axis of its intended position. This feed rate is calculated by the gcode behavior based on the maximum feed rate of the extruder and the maximum feed rates of the X-Y axes.



The gcode behavior will then flush the serial transport so that it can buffer the next command. 



At this point - the serial transport, extruder and axes are independently performing their operations as the event loop is servicing them. The next command is held buffered until the gcode behavior receives the cartesian mechanism  �� ��s   ��˜destination complete  �� �� notification.



When the cartesian mechanism has been notified by each axis that they have reached their destination, then it can notify the gcode behavior the cartesian mechanism has finished its command, allowing the gcode to service the next command if available.



When the position of an Axis is not at the destination location, it sets the motor direction and instructs it to   ��˜move  �� �� at a certain speed. If the axis is built using stepper motors, the motor device registers for periodic notifications from the event loop, the period is defined by the feed rate and stepper configuration. During each trigger from the event loop, it will step the motor.



The End stops are an interesting device. This device could be implemented with an interrupt or poll using a periodic event on the event loop. In either case, when the end stop triggers, observers are notified - in this case the axis. In response, the axis will pause the motor and notify its observers of an   ��˜axis end reached  �� �� event.



### Switching it up


While this may seem like tons of overhead, the benefit comes from the ability to switch out portions of code for new mechanisms or devices which conform to the interface. 



The Axis mechanism could be augmented with a position sensing device (like an encoder), which enhances the accuracy of the axis, but is an independent improvement that does not fundamentally change the interface or require changes elsewhere in the code base. 



The Axis mechanism could be completely redesigned with a linear actuator - an independent improvement which negates the need for the majority of the devices of a traditional axis, but does not require changes to other parts of the code base.



Similarly, an Extruder may be implemented using an brushed motor, a stepper motor, or linear actuator. Since it is up to the device implementation to conform to the interface, the extruder mechanism need not manually manage the details of the motor itself. 



### Off Chip


One benefit of using the interface pattern, is that the interface implementation can be local to the microcontroller, but the device implementation can live off chip. With the event driven design as recommended here, the interface can communicate asynchronously with the off chip devices without disturbing other components in the system.



### Next Steps


I'm currently building the support systems and generic pattern implementations for the Wiring, which is independent of the RepRap code base. I'm building a suite of tests which verify the implementation, as well as emulating portions of the Wiring APIs. I'll check these into my user area in the RepRap source tree. I'll continue to post portions of the code as they come online - First the Dynamic array implementation, then the event loop with timers and callbacks, then device abstraction, and finally the RepRap refactor.


