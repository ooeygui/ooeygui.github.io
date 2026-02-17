---
layout: post
title: "avr-libc - Realloc bug"
date: 2009-01-30 22:30:47
categories: [Random, 3D Printing]
wordpress_id: 216
redirect_from: /?p=216
---

A few months ago I proposed a refactor for the Arduino RepRap firmware. I was quick to code it up and build test cases on the desktop, then ported to the Arduino... Where it failed miserably. There the code languished as I spent time on it here and there. I attempted getting SimulAVR working, but it doesn't support the Arduino chipsets (thought about adding support). I tried AVR Studio in a bootcamp'd windows install, but it kept hanging when debugging this problem. As a last resort, I ported the avr-libc memory apis to the initial desktop refactor, and was able to see the problem clear as day. I spent some time reducing the repro to the smallest form:

```
void testMalloc()
{
    size_t* array = (size_t*)malloc(4 * sizeof(size_t));
    free(array);
    array = NULL;

    array = (size_t*)realloc(array, sizeof(size_t));
    array = (size_t*)realloc(array, 2 * sizeof(size_t));
    array = (size_t*)realloc(array, 3 * sizeof(size_t));
    realloc(array, 4 * sizeof(size_t));
}
```


There is a bug in the free list manager in Realloc, specifically when growing a buffer into the next free entry. I was convinced I had a bug in a fairly large codebase, and whittled it down to this reproduction. I'm now playing with a couple of fixes, but need to figure out how to get a 'blessed' fix into lib-avr.

Stepping through the malloc into the free results in:
```

00000010 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000

```


Over the first realloc:
```

00000008 00000000 00000000 00000004
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000

```


Over the third:
```

0000000c 00000000 00000000 00000004
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000

```


And finally:

```

00000010 00000000 00000000 00000004
00000000 FFFFFFFC 00000000 00000000
00000000 00000000 00000000 00000000

```

At this point the free block pointer (__flp) points to the second line, with a size of 0xfffffffc. The next allocation fails.
