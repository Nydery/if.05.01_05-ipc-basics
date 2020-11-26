### if.05.01 TINF Operting Systems

# Assignment 5: Basics of Interprocess Communication
## Objective
The goal of this week's assignment is to make you familiar with basic terms and questions concerning IPC.

## Required Tasks
Answer the following questions in a md file which you add to this repository.

1. **Race Condition**
Describe with your own words: What is a race condition?

2. **Disabling Interrupts**
   1. Why is it impossible to achieve Mutual Exclusion via disabling interrupts on a multi-core machine?
   2. Why is it dangerous to give user processes the power to disable interrupts?
   
3. **Peterson's Solution**
	1. Play through the two scenarios of the handout of Peterson's solution. Document how it works.
	2. Play through the scenario which makes the strict alternation approach fail. Document how it fails.
	3. What is the meaning of the variable `loser` in Peterson's solution? When does it have any effect?
	4. Extend the given functions such they can handle three processes.
