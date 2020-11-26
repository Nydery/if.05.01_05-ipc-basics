# 1) Race Condition

## Description:
A race condition is considered a situation where, for example, two processes/threads want to access a shared variable at the same time. In this situation you are most likely to lose some information, due to the fact that those two processes can't run 100% parallel. Or in other words, both processes would write to the same address, which would result in a loss of data. **This example only applies to SINGLE-CORE processors.**



# 2) Disabling interrupts

1) The reason why interrupts **DO NOT** work on multi-core machines is that, as it already says, it has multiple cores. All of those cores can run in parallel without any problems. When you disable interrupts with your application, you only disable them on the core it is running on right now. Therefore, any other application running on a different core, can still access the shared resource. 

2) It can be **VERY DANGEROUS** to give the user the power to disable interrupts, because if the user somehow forgets to enable the interrupts again, the PC is useless. The scheduler can't interrupt and therefore no other application or service (keyboard input, background services, and so on ...) can run. That way you can't even shutdown the PC, because that is done with an executable file as well. 

# 3) Peterson's Solution

## Implementation

```java
int loser;
boolean[] interested = new boolean[2];

void enter_region(int process){
	int other = 1 - process;
	interested[process] = true;
	loser = process;
	while(loser == process && interested[other]);
}

void leave_region(int process){
	interested[process] = false;
}
```



## 1) First scenario: 

1) Process 0 calls enter_region(), finishes it and is within the critical region. 

**Now the array interested looks like this [true, false] and loser is equal to 0**.

2) Process 1 calls enter_region()

**Now the array interested looks like this [true, true] and loser is equal to 1. As you can see in the implementation above, the process entering enter_region() must wait until loser != process or the other process is not interested anymore. Which is not the case at this point.**

3) Process 0 calls leave_region()

**Now the array interested looks like this [false, true] and loser is equal to 1. Next time process 1 gets some runtime, it can continue and enter the critical region.**



## 2) Second scenario

1) Process 0 calls enter_region() and gets interrupted before it is finished executing the while loop.

**Now the array interested looks like this [true, false] and loser is equal to 0.**

2) Process 1 enters enter_region() and finishes before it gets interrupted

**Now the array interested looks like this [true, true] and loser is equal to 1. The process couldn't enter the critical region because of the loop condition.**

3) Process 0 continues executing enter_region().

**Now the array interested looks like this [true, true] and loser is equal to 1. Process 0 enters the critical region.**

....



## 3) Meaning of loser variable

The loser variable represents the process that was last to call enter_region() and affects the while loop condition so that only one process is in the critical region at a time. 

## 4) Handle 3 processes

The implementation for three processes would look like this:

```java
int loser;
boolean interested = new boolean[3];

void enter_region(int process){
	int other, other2;
    switch(process){
        case 0:
            other = 1;
            other2 = 2;
            break;
        case 1:
            other = 0;
            other2 = 2;
            break;
        case 2:
            other = 0;
            other2 = 1;
           	break;
    }
    
	interested[process] = true;
	loser = process;
	while(loser == process && (interested[other] || interested[other2]);
}

void leave_region(int process){
	interested[process] = false;
}
```



