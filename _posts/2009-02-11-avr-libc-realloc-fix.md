---
layout: post
title: "avr-libc realloc 'fix'"
date: 2009-02-11 23:34:53
categories: [Random, 3D Printing]
wordpress_id: 221
redirect_from: /?p=221
---

I've been working on patterns for Arduino, which relies on a dynamic array for managing event loops, observables, and device abstraction. However, I was blocked by a critical failure in realloc. While I could have worked around it using malloc/free, but wanted to understand what was going on in libc.



One thing that struck me about realloc was the naming conventions; What was the difference between fp1, fp2, cp1, cp2? Some referred to a free block, some referred to a free pointer. I gave up and renamed the variables to something more readable.




The fatal flaw has to do with mixing sizeof(__freelist) and sizeof(size_t). In many cases, the difference results in indexing into the middle of a free list entry, thus corrupting memory.




(compare to [avr-libc/libc/stdlib/realloc.c](http://cvs.savannah.gnu.org/viewvc/avr-libc/libc/stdlib/realloc.c?root=avr-libc&view=markup))



```
void* realloc(void *ptr, size_t newLength)

{

    struct __freelist *currentBlock, *nextBlock, 

        *currentFreeBlock, *previousFreeBlock;

    char *currentPointer, *nextPointer;

    void *memp;

    size_t largestBlockSize, sizeIncrease;

    

    /* Trivial case, required by C standard. */

    if (ptr == 0)

        return Malloc(newLength);

    

    currentPointer = (char *)ptr;

    currentBlock = (struct __freelist *)

         (currentPointer - sizeof(size_t));

    

    nextPointer = (char *)ptr + newLength; /* new next pointer */

    if (nextPointer sz) 

    {

        /* The first test catches a possible unsigned int

         * rollover condition. */

        if (currentBlock->sz  currentBlock->sz - sizeof(struct __freelist))

        {

            return ptr;

        }

        

        nextBlock->sz = currentBlock->sz - newLength - sizeof(size_t);

        currentBlock->sz = newLength;

        Free(&(nextBlock->nx));

        return ptr;

    }

    

    /*

     * If we get here, we are growing.  First, see whether there

     * is space in the free list on top of our current chunk.

     */

    sizeIncrease = newLength - currentBlock->sz;

    currentPointer = (char *)ptr + currentBlock->sz;

    for (largestBlockSize = 0, previousFreeBlock = 0, 

         currentFreeBlock = __flp; currentFreeBlock; 

         previousFreeBlock = currentFreeBlock, 

             currentFreeBlock = currentFreeBlock->nx) 

    {

        if (currentFreeBlock == nextBlock && 

            currentFreeBlock->sz >= sizeIncrease) 

        {

            /* found something that fits */

            if (sizeIncrease sz + sizeof(size_t))

            {

                /* it just fits, so use it entirely */

                currentBlock->sz += 

                    currentFreeBlock->sz + sizeof(size_t);

                if (previousFreeBlock)

                    previousFreeBlock->nx = currentFreeBlock->nx;

                else

                    __flp = currentFreeBlock->nx;

                return ptr;

            }

            

            /* split off a new freelist entry */

            currentPointer = (char *)ptr + newLength;

            nextBlock = (struct __freelist *)

                (currentPointer - sizeof(size_t));

            nextBlock->nx = currentFreeBlock->nx;

            nextBlock->sz = currentFreeBlock->sz -

                 sizeIncrease - sizeof(size_t);

            

            if (previousFreeBlock)

                previousFreeBlock->nx = nextBlock;

            else

                __flp = nextBlock;

            currentBlock->sz = newLength;

            return ptr;

        }

        

        /*

         * Find the largest chunk on the freelist while

         * walking it.

         */

        if (currentFreeBlock->sz > largestBlockSize)

        {

            largestBlockSize = currentFreeBlock->sz;

        }

    }

    /*

     * If we are the topmost chunk in memory, and there was no

     * large enough chunk on the freelist that could be re-used

     * (by a call to malloc() below), quickly extend the

     * allocation area if possible, without need to copy the old

     * data.

     */

    if (__brkval == (char *)ptr + currentBlock->sz && newLength > largestBlockSize) 

    {

        nextPointer = __malloc_heap_end;

        currentPointer = (char *)ptr + newLength;

        if (nextPointer == 0)

        {

            nextPointer = STACK_POINTER() - __malloc_margin;

        }

        

        if (currentPointer sz = newLength;

            return ptr;

        }

        

        /* If that failed, we are out of luck. */

        return 0;

    }

    

    /*

     * Call malloc() for a new chunk, then copy over the data, and

     * release the old region.

     */

    if ((memp = Malloc(newLength)) == 0)

        return 0;

    memcpy(memp, ptr, currentBlock->sz);

    Free(ptr);

    return memp;

}




```
