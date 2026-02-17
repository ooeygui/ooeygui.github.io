---
layout: post
title: "Event Loop for the Arduino"
date: 2009-03-27 09:36:39
categories: [Random, 3D Printing]
wordpress_id: 293
redirect_from: /?p=293
---

Making an LED blink or driving a single stepper using the Arduino is very easy - pulse a pin in the Arduino loop handler is all you need. However, as builds become more complex, there is often the need to manage multiple devices.

The RepRap firmware is such a project. The firmware has numerous steppers, motors, heaters, fans and sensors - it is chalk full of features. The developers have literally performed magic with the tiny little Arduino. However, I believe they will admit that the code base is becoming a little unwieldy and difficult to modify.

As an operating systems developer, my job is to identify patterns in application development, build abstractions which simplify these common cases and generate boiler plate or error prone code. In my post on the [RepRap firmware refactor](https://ooeygui.com/?p=143), I described several design patterns which are applicable to not just RepRap, but many other complex embedded projects. The best place to start is with the root of all evil - the event loop.

### Event Loop

Event driven programing is a staple of modern application development. In my experience, event driven patterns are not as prevalent in embedded development - it's a shame, because it is very useful. The main conceptual leap requires the developer to give up control to the power of the loop - a leap that is often very difficult as they are used to complete control. I've completed an implementation, which in its current state is more of a timer loop, but will grow to enable queued events (both on and off board).


This code is currently checked into [the RepRap source forge repository](https://reprap.svn.sourceforge.net/svnroot/reprap/trunk/users/lamadio/FirmwareRefactorPrep/main/). It is only dependent on the Dynamic Array implementation checked into the tree as well.

There are several features of this event loop:

- Periodic (or cycle) events

- Timed events, with support for wrapping (since the milliclock quickly overflows)

- Reentrant - You can add or remove events while processing an event

- Support for nested, independent Eventloops (for those special cases)



(NOTE: It is not interrupt safe at the moment)

### Sample Use

```
void StepperDevice::turn(float numberOfRevolutions)
{
    if (_currentTick)
    {
        stop();
    }
    
    _targetTick = (int)(numberOfRevolutions / _ticksPerRev);

    notifyObservers(StepperEvent_Start, this);
    EventLoop::current()->addTimer(this);
}

void StepperDevice::fire()
{
    digitalWrite(_stepPin, HIGH);
    delayMicroseconds(5);
    digitalWrite(_stepPin, LOW);

    ++_currentTick;
    
    if (_targetTick && (_currentTick == _targetTick))
    {
        notifyObservers(StepperEvent_Complete, this);
        stop();
    }
}
```



### The Event Loop Header

```

#ifndef EventLoop_h
#define EventLoop_h

typedef unsigned long milliclock_t;
extern const unsigned long MILLICLOCK_MAX;

//
// Periodic Event Callback
//  Derive from this class to implement a periodic servicing
//
class PeriodicCallback
{
public:
  // NOTE: This is a harmless warning: 
  //  alignment of 'PeriodicCallback::_ZTV16PeriodicCallback' 
  // is greater than maximum object file alignment.
  // Bug in avr-g++. 
  // See http://www.mail-archive.com/avr-chat@nongnu.org/msg00982.html
    PeriodicCallback();
    virtual ~PeriodicCallback();
    
    virtual void service() = 0;
};

//
// Timer
//  Derive from this class to implement a periodic timer.
//  This class also contains information needed for 
//  maintianing a timer, designed to be memory efficient.
//
class EventLoopTimer
{
private:
    milliclock_t _lastTimeout;
    milliclock_t _period;
protected:
    inline void setPeriod(milliclock_t period) 
        { _period = period; }
        
public:
    EventLoopTimer(unsigned long period);
    virtual ~EventLoopTimer();
    
    virtual void fire() = 0;
    
    inline milliclock_t period() const 
    { return _period; }
    inline milliclock_t lastTimeout() const 
        { return _lastTimeout; }
    milliclock_t nextTimeout() const;
    
    inline void setLastTimeout(milliclock_t nextTimeout) 
        { _lastTimeout = nextTimeout; }
};

//
// Event Loop
//  This class implements the main loop. 
//  It allows clients to register for periodic 
//      servicing, or timed servicing.
//  
class EventLoop
{
private:
    DArray _periodicEvents;
    DArray _timers;
    milliclock_t _lastTimeout;
    bool _running;
    
    void sortTimers();
    DArray* findFiringTimers();
public:
    EventLoop();
    ~EventLoop();
    
    void addPeriodicCallback(PeriodicCallback* callback);
    void removePeriodicCallback(PeriodicCallback* callback);
    
    int periodicCallbacks() { return _periodicEvents.count(); }

    void addTimer(EventLoopTimer* timer);
    void removeTimer(EventLoopTimer* timer);
    int timers() { return _timers.count(); }
    
    bool running() { return _running; }
    void exit() { _running = false; }

    void run();
    
    static EventLoop* current();
};

#endif
```
