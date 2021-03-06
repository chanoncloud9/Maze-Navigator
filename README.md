# Maze-Navigator
Real-Time Embedded Systems

### Project 
Maze navigation

### Algorithm
- We tried to create a simple yet robust algorithm in order to ensure that the vehicle can navigate through most mazes with relative simplicity.
- We were able to achieve close to perfect turns, and we believe that we can perfect them further with more tweaking of various parameters like delays.
- We made use of the concept of alignment to ensure that we are able to move the vehicle smoothly with precision to ensure the vehicle doesn’t deviate from its path beyond a certain acceptable limit.
- We did not make use of encoders in order to perform turns. In lieu of it, we made use of a strategy to constantly have a pattern of turning and moving forward incrementally to ensure that we were able to make different radius turns as required by a maze while navigation. Using such a technique we were able to create a trajectory of sorts to ensure that we turned correctly each time.
- While making use of the provided ultrasonic sensors, we were faced with the problem of erroneous(garbage) values being given at certain times. In order to mitigate this issue, we made use of a ceiling limit to mimic a low pass filter (which allows values within a certain set limit to pass and discard all values above it) so that the operation of the vehicle would not be impacted by these garbage values.
- We made use of PWM in order to ensure that we we're able to run the vehicle(motors) below the 100% power and ensure better performance by giving a set duty to the motors while turning, moving fwd etc. While using the Arduino IDE, one challenge we encountered was that the PD3 pin for one motor would not respond to the PWM code. On delving into the pin_map for the STM32F3 in the discovery_f3.cpp file, we found that there was no timer/channel mapped to the PD3 pin. We changed the pin_map to accordingly reflect the timer/channel and hence solved the issue.
- For this entire project we made use of the STM32 Arduino IDE which was introduced to us in class.
- Also, in order to implement PWM within the code we referenced the article 'Using PWM to drive Servos'. We had to make changes to the method being demonstrated in the article in order to use it within our IDE. The original article can be found at: http://www.kaltpost.de/?page_id=412

### Code Walkthrough
- First, we initialized the pins for the ultrasonic sensors, motor_start_flag, stop flag etc. giving them aliases to be used later
- Then defined functions for the motor movement: 1. forward_left
2. motor forward 3. motor_turn_180
- We created an align function to ensure that the vehicle doesn’t deviate from the wall too much, and in that case does perform path correction and comes back on track. This has been done using the align_r and align_l functions.
- Wall function is a function within which we have implemented the actual logic/algorithm for our maze navigation and it is called within the code, whenever we meet the conditions which ensure that the vehicle is aligned to the walls.
- To start the operation, we have a setup function within which we initialize the baud rate as 9600 Hz, and then setup the ultrasonic sensor pins to INPUT/OUTPUT for all the three sensors.
- Once, everything is set up, we run a function called loop. This is intended to be run throughout the duration of operation.
1. Get the distances from the front and right walls and store them in appropriate variables making use of the ultrasonic function.
2. If the distance on right side is lesser than 2, we will perform alignment on the left hand side as the vehicle is too close to the right side wall.
3. Align_l1 has been created as a function to ensure that we handle garbage values being thrown by the ultrasonic sensors i.e. distance values greater than 200.
4. If right side distance is greater than 3 and lesser than 8, perform right side alignment to ensure that the vehicle doesn’t stray too left.
5. Otherwise call the Wall function.
- An 'ultrasonic' function was created in order to ensure how much delay we are taking in between readings and update the right and front distances. We had to play around with these distances to get the perfect one which would work for our cases.
- We created functions for the motion of the motor 1. Motor Stop
- pass 0 to all the motor pins to ensure they stop 2. Motor Forward
- pass the duty to the pins PD7 and PD4 to ensure they are running with the specified duty cycle for the PWM.
3. Motor Reverse

- pass the duty to the pins PD6 and PD3 (The reverse pins) to ensure they are running with the specified duty cycle for the PWM.
- The above operations are performed by passing the values of duty to the motor function which taken in the pin and the duty to be supplied to it. It is within this function that we perform the PWM operations and give the input duty to the pin for the motor operation.
- For the functions which actually make the vehicle turn we have three main functions: 1. forward_right
- Within this function we are moving our vehicle forward with a given duty cycle. The delay being set in this function is used to help turn the vehicle while executing the code. We chose to give the two motors a different duty so that *******
2. forward_left
3. motor_turn_180
- The align functions which we have created to ensure that the vehicle doesn’t stray too far from its path and remains close to the walls is also made in three versions:
1. align_r 2. align_l 3. align_l1
- The wall function is the function which is implementing the core functionality of the maze navigation.

### Video
https://www.youtube.com/watch?v=AvfczuB6AUY
