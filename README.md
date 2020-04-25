# N-Number-Gcd-Finder-Single-Purpose-Processor



This single-purpose processor is designed for finding the greatest common divisor of maximum 256 16-bits of numbers at the same time.
The algorithm steps taken through it are as follows:
* 1) Implementing a C code that accomplishes this.
* 2) Designing the finite state machine of the C code.
* 3) Designing a datapath based on the finite state machine.
* 4) Designing a transition diagram based on the datapath.
* 5) Creating a transition table based on the transition diagram.
* 6) Deriving equations, that input to datapath and state flip flops, from the transition table.
* 7) How to use it?


## Implementing a C code that accomplishes this

The C code below takes n numbers from user and finds the greatest common divisor based on Euclid algorithm. The code has been tested with various test cases in order to make sure nothing is wrong with it.

![image](https://user-images.githubusercontent.com/23126077/80269420-66802500-86b8-11ea-8333-ff66a6d2c7dd.png)


## Designing the finite state machine of the C code.

Any time a number is entered, it is written into RAM immediately after the button click. RAM keeps 16-bits of numbers in the addresses starting from 0x00 to 0xff. After writing, the greates common divisor of the first two number is computed and the result is written into res register. Then, the greatest common divisor of the next number and the content in the res register is computed and this goes like this until all the numbers in the RAM are consumed. Finite state machine of the C code is as follows:

![image](https://user-images.githubusercontent.com/23126077/80269603-cb884a80-86b9-11ea-8486-17934f8a183a.png)


## Designing a datapath based on the finite state machine

The datapath below is designed with evaluating the every state's content and necessary signals. Datapath outputs some signals to control unit so that control unit can send some signals to datapath for transitioning the new state.

![image](https://user-images.githubusercontent.com/23126077/80269718-cbd51580-86ba-11ea-9248-da21317b2f4c.png)


## Designing a transition diagram based on the datapath

After identifying which signals should come in any state, new finite state machine with select signals only is designed as follows:

![image](https://user-images.githubusercontent.com/23126077/80269748-12c30b00-86bb-11ea-9e10-114441a53f58.png)


## Creating a transition table based on the transition diagram

Signals that transite from one state to another and that are used once transited are added based on the finite state machine with signals only below in a table form. This table is used to derive equations of which signals sent to the datapath in the next state.

![image](https://user-images.githubusercontent.com/23126077/80269834-dba12980-86bb-11ea-8524-023df78fdaec.png)


# Deriving equations, that input to datapath and state flip flops, from the transition table.

Equations of signals mentioned right above are as follows: 
Note: Since equations of the state flip flops are too long, signals S0, S1, ..., S11 are created, one for every possible state in the finite state machine.

```
S0 = P3’P2’P1’P0’	      
S1 = P3’P2’P1’P0	      
S2 = P3’P2’P1P0’	      
S3 = P3’P2’P1P0	
S4 = P3’P2P1’P0’	      
S5 = P3’P2P1’P0	        
S6 = P3’P2P1P0’         
S7 = P3’P2P1P0	
S8 = P3P2’P1’P0’	      
S9 = P3P2’P1’P0	        
S10 = P3P2’P1P0’	      
S11 = P3P2’P1P0	
en_ram = S5 + S3 + S7	  
m5 = S9	                
m1.1 = S7	              
m1.0 = S3	           
ld = S5 + S7
m3 = S11	             
en_k = S1 + S11	        
en_i = S1 + S4	        
m4.1 = S10	         
m4.0 = S7
en_res = S5 + S9	      
en_out = S1 + S7 + S10	
m6 = S6	                
m7 = S9	             
m2 = S4
N3 = S7 + S8 k1’ b1’ e1 + S8 e1’ (k1 xor b1) + S9 + S10
N2 = S2 k2’ e2 + S3 + S5 + S6 k2 + S11
N1 = S1 + S2 k2 e2’ + S4 + S5 + S6 k2 e2’ + S8 b1’ (k1 xor e1) + S11
N0 = S0 go + S2 (e2 xor k2) + S6 k2 e2’ + S8 k1’ b1 xor e1)
```


## How to use it?

After opening the gcd.circ file, followings are needed to be done:
* Number of inputs should be fed into the input pin '# of NUMBERS'
* Clock signal should be started(ctrl + k)
* Since the setup is ready, the circuit waits for a high go signal. Click go button.
* The first number whose greatest common divisor is found should be written into input pin 'NUMBER'.
* Click Input button.
* Repeat the last two instructions '# of NUMBERS' times.
* Since all the numbers are written into RAM now, click Input button once more and let the calculate begin!

Note: After the computation, the processor does not start again unless the states are initiated manually. Because of the clock signal it is fed into the processor, the circuit can't transite from state zero to two.

## Built With
* [Logisim 2.7.1](http://www.cburch.com/logisim/)
