---
layout: post
title: "Behavior - Device Abstraction"
date: 2009-03-28 09:42:47
categories: [Random, 3D Printing]
wordpress_id: 301
redirect_from: /?p=301
---

You've coded your project with your x-axis step pin on GPIO11. But my device has it on GPIO3. Or even worse, I use a a servo and counter, which doesn't have a concept of a step. How do we resolve this?


If instead of considering the device itself when coding the logic, what if we were to abstract the meaning of the device from its behavior? That's the basic idea behind Behavior-Device abstraction.

### Navigation sample of the idea


Say I'm building a Unmanned Aerial  Vehicle (UAV). I have a navigation behavior which needs realtime orientation data. A typical orientation circuit requires a Gyro and accelerometer for accuracy and drift compensation. If the navigation system had to deal with those calculations, that code becomes unwieldy. However, if it were coded to a single OrientationDevice, then the implementation of that device could be two sub devices - the GyroDevice and AccelerometerDevice, and can build a homogenized notification of orientation changes. What if a manufacturer were to build a single chip that handles both and provides a single interface to it? All one would need to do is replace out the Orientation device without rewriting the Navigation behavior.


The example above described a composition - Each component has a singular responsibility. These are aggregated into a composite or logical device, which then can be actioned upon or notified from independent of the internal implementation.


### GCode behavior - Cartesian bot/Extruder


RepRap has 1 main behavior - The GCode interpreter. It receives commands from the host machine, and drives two logical components - the extruder and the Cartesian bot. The Cartesian bot is composed of 3 linear actuators. The Extruder has a temperature sensor, heater and feed device.


The RepRap project uses Stepper motors for linear actuation, with end stops for collision avoidance and homing. It could be implemented using a servo motor or linear stepper driver. Since driving the linear actuator is now abstracted from the behavior, changes to the driving mechanism becomes much less complicated.


In other words, the behavior tells the bot where to go, the bot figures out how to get there, and the linear actuators figure out how to do it.


#### The Cartesian Bot


```
class CartesianDevice : public Device, 

                         public Observable,

                         public Observer

 {

     LinearActuator& _x;

     LinearActuator& _y;

     LinearActuator& _z;

     bool _xInMotion;

     bool _yInMotion;

     bool _zInMotion;

public:

     CartesianDevice(LinearActuator& x, LinearActuator& y, LinearActuator& z);



     void moveTo(float newX, float newY, float newZ);

     void moveHome();

     inline bool axesInMotion() { return _xInMotion || _yInMotion || _zInMotion; }

     virtual void notify(uint32_t eventId, void* context);

 };
```





Linear Actuator</h4

```
class LinearActuator : public Device, 

                       public Observable,

                       public Observer

{

    float _currentPos;

    float _revPerMM;

    StepperDevice& _stepper;

    OpticalInterrupt& _nearInterrupter;

    OpticalInterrupt& _farInterrupter;

    

public:

    LinearActuator(float _revPerMM, StepperDevice& stepper, 

      OpticalInterrupt& far, OpticalInterrupt& near);

    inline float currentPosition() { return _currentPos; }

    inline void setTempRate(float rate) { _stepper.setTempRate(rate); }

    void moveTo(float newPosMM);

    void moveHome();

    virtual void notify(uint32_t eventId, void* context);

};
```





#### Stepper Device


```
class StepperDevice : public EventLoopTimer, 

                      public Device, 

                      public Observable

{

    int8_t _stepPin;

    int8_t _dirPin;

    bool _forward;

    int _currentTick;

    int _targetTick;

    int _ticksPerRev;

    milliclock_t _maxRate;

public:

    StepperDevice(int8_t stepPin, int8_t dirPin, int ticksPerRev, milliclock_t rate);

    

    void goForward();

    void goBackward();

    void turn(float numberOfRevolutions = 0.0f);

    void start();

    void stop();

    void setTempRate(float rate);

    virtual void fire();

};
```





#### OpticalInterrupt


```
class OpticalInterrupt : public Device, 

                         public Observable, 

                         public PeriodicCallback

{

    int _inputPin;

public:

    OpticalInterrupt(int pin);

    virtual void service();

};
```
