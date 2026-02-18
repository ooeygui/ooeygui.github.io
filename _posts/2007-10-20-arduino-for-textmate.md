---
layout: post
title: "Arduino for TextMate"
date: 2007-10-20 20:07:31
categories: [Electronics]
wordpress_id: 46
redirect_from: /?p=46
---

I've been playing with the [Arduino Microcontroller](http://arduino.cc/) for quite some time. It is a fantastic open source hardware project which really opens up the world of Microcontrollers to people all over the world. The developers on the project have included a development environment written in Java. I'm a huge fan of [TextMate](http://www.macromates.com). I decided to write a plugin for TextMate which allows you to:



	-  choose the microcontroller architecture - Atmega 8, Atmega 168, and the Make Controller (forthcoming).


	-  Compile and upload to the Microcontroller
	

	-  Show a Serial Monitor using the OSX terminal





### Installation
UPDATE: This has been modified for the current release
 Install the Arduino 0010 release from arduino.cc.


download: [Microcontroller.tmplugin](/assets/downloads/Microcontroller.zip)


unzip it to ~/Library/Application Support/TextMate/Plugins.

 ### Building
When you build from TextMate, you need to do a few things differently. First, include wprogram.h, and add a main loop. Here's the template:

```


#include 

void setup()

{

}

void loop()

{

}

int main()

{

	init();

	setup();

	while (1)

		loop();

	return 0;

} 


```


### About the binary
I'd consider this a .1 release; It hasn't been well tested. Because of this, there will be bugs.

### Future Enhancements




	-  User specified makefile (something that gets included into the build process instead of manually walking the tree).


	-  Make controller support.


	-  Hot Keys for building


	-  Don't leak so much





			

		### Feedback
If you'd like to send feedback or request features, send them to louie at ooeygui.
