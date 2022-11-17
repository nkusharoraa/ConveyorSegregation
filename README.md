# ConveyorSegregation
Automated Sensing and Segregated Distribution of Variable Length Boxes on Different Conveyors

With the help of the vertical switching point, two types of boxes are to be fed on two different conveyors. The destination of the swiveling slide (upper or lower) is decided by means of two proximity sensors. The upward motion of the double acting cylinder (1A). If the Part is of Type 1 it must be fed on the upper conveyor while Type 2 parts are to be fed on the lower conveyor. The HMI must have a start button for starting the conveyors. Two selector switches are present on the HMI. One is for sensing the parts on the conveyor and the other for pushing Type 1 and Type 2 boxes on the conveyor.

![Screenshot 2022-11-17 230827](https://user-images.githubusercontent.com/59050030/202518174-fb51febf-7f4e-4d98-a23f-97d2e5c223b2.jpg)

Figure 1: The desired Automation

## 1. Brief explanation

Once we press the boxes, sensors and set the Toggle switch on either PLC or EPN and select the Type of box then we will press the Start conveyor. After that our box will start moving and if we set the Type2 then the box will go to the lower conveyor and the cylinder will not actuate but if our Type is set to 1 then the cylinder will actuate and connect the upper conveyor and Type1 boxes will go onto the Upper conveyor. We can also switch the toggle switch between PLC or EPN (Electro-Pneumatics) that will decide on which logic circuit our virtual system will work.
Once we start the conveyor and our box is moving then if we change the Type then first the current box will go complete till end and only after that new box of selected Type will come. Here, we can also control the Speed of the conveyor using a slider (speed). We can also stop the conveyor using the speed slider. Reset button set every variable is zero that we made for our debug purpose.
Sensors will only sense if Sensors Toggle switch is on (that we can ensure by LED light) and if both sensors are sensing at the same time then Cylinder will actuate and connect to the upper conveyor. And Boxes will only come if Boxes Toggle switch is on (that also we can ensure by LED light). Stop button is able to stop the conveyor and movement of the box on the conveyor, not on the plate. An Emergency button is able to stop everything in the Virtual system (like Sensors, Boxes, conveyors) except the movement of the box on the plate because of gravity falls.


## 2. Sequence of operations:

To achieve the desired motion, we added a few basic components, including the sensor, the cylinder, two conveyors, and a slide, as required. The two boxes of differing lengths are concealed behind a curtain and pushed when the respective buttons are touched. Essentially, we are referring to the following buttons: Start, Stop, Reset, Type1/Type2, EPN/PLC, Sensors, Boxes, and a Conveyor Speed slider. The majority of names are self-explanatory.  Once we have used the boxes and sensor button to make the boxes appear and the sensors function, the color of the small window close to the curtain will change. When we add a button that allows the user to select the type of box, we activate it by adding one to the variable that represents the box's horizontal position. Consequently, the related length variable is either set to 20 or 40 depending on whether the Type1 or Type2 button is selected on the HMI. Now, the sensor on the left will activate when a box (such as the smaller one) approaches it. As a consequence, a green light will show. The light on the sensor will switch off now that the box has completely passed it (even the back end of the box is no longer under the sensor). If the box is larger and remains stationary until it reaches the correct sensor, both sensor lights will be on, and we will receive a signal to push the cylinder. The operation of both sensors indicates the length variable of the box within the SFC, and the cylinder is pushed based on this data (it will come down if the box is smaller and it was already in a pushed position). This is done in order to boost the work's productivity. We can return the boxes to their initial condition at any time by hitting the reset button. If the stop button is pressed, the box will remain in the same position on the conveyor as it was when the conveyor was stopped. When the emergency stop is engaged, both the conveyors and the sensors are stopped.
When the box reaches the right end of the upper conveyor, we wait until the rear end of the box (which is determined by manually checking the position of the box variable showing its horizontal position) is no longer in contact with the conveyor. This enables us to increase the rotation variable of the box by identifying the center of revolution as the bottom left corner of the box, which is the last point of contact between the box and the conveyor. The increment is maintained at the same level based on the predetermined degree of slider rotation. After completing its rotation, the motion is fed by gravity, and we set it so that the motion speed increases in proportion to the slider's inclination. It is vital to note that we cannot use the "stop" button to prevent the movement of our box at this place. This is due to the fact that it is fed by gravity, which helps to conserve electricity during operation. The motion along the slides is maintained with the use of a variable representing the box's vertical position. This variable's value increases (which is positive while moving in the opposite direction of the incline), hence sustaining the motion.
Intriguingly, the rotation is designed to maintain two points until the box hits the bottom, after which the box's vertical position is maintained. Now, it is to be noted that the stop button is active, as the box is moving towards the right on the conveyor belt.

## 3. Creativity aspect of our project:

The main aspects where we introduced creativity are: 

### 3.1 Speed offset slider:

A speed slider was included to change the speed of conveyor belts. The slider can be set to either increase or decrease the speed of these conveyors on a multiple basis scale. The middle position thus, sets the conveyor to a pre calibrated/ factory speed, then the slider can be turned down to decrease the speed of the conveyer belt and alternatively, the slider can be turned up to increase the speed of the box. 
The speed slider can thus also be used to increase or decrease the simulation time. However, the speed of gravity feeding mechanism, connecting the two conveyors, is not affected by the slider for obvious reasons.  This shows that, in a practical sense, the gravity feed step will be a limiting variable while trying different conveyor speeds for varied applications. 

### 3.2 Box-Type Change During Cycle:

Box-type, when changed mid simulation allows the current box to reach the end, then changes the type before the appearance of the next box. This is inline with actual industry practices, in which most machines do not stop/hamper transport, if a wrong box is selected/sent.

### 3.3 PLC & EPN switchover using selector switch:

We included a selector switch to run the system using either PLC or EPN control circuits. 

### 3.4 Emergency stop button:

The E-Stop button, when pressed, stops all power to the machine assembly, that is, it stops all conveyors and sensors. The boxes, if on the conveyor, stop too and new boxes do not appear. If, however, the boxes reached the gravity feed plate before E-Stop was pushed, they would slide till the bottom and then stop on their assigned conveyor.

### 3.5 Efficient logic for mechanism:

Cylinder remains extended till Type 1 box is coming and goes down only when it detects Type 2 box and vice versa

### 3.6 Selector switches to start sensors and boxes
We included selector switches to start the sensors and box delivery onto the conveyor. If the sensors aren’t started, the gravity feed plate doesn’t move and all boxes are sent to the lower conveyor. Box delivery only starts when the switch is turned on(and not on conveyor start) and continues automatically till the button is pushed off, sending a new box each cycle.

## 4. Project Deliverables:

### 4.1 Virtual System with HMI controls:

#### Virtual system variables

start_box, start_boxp, start_boxe, PLC_EPN2, start_sense, start_sensep, start_sensep, start_conv, start_convp, start_conve, stop, stopp, stope, conv_rotate, conv, E_stop, reset, length, Type, box, box_y, box1, box1_y, box_r, box1_r, cyl, plate, 1Y2, 1Y4, sensor1, sensor2, 1Y1, 1Y3 (Created in SFC).

#### HMI variable

Push buttons are Start conveyor, stop, reset, emergency; Toggle switches are PLC-EPN, Sensors, Boxes, Type1-Type2 and one Slider button is Speed button.

#### Input variables:

start_boxp and start_boxe: Both are boolean variables. Once we press the Boxes button (created in HMI) and the stop button is not pressed then start_boxp will be set to 1 using PLC logic otherwise start_boxp will be set to 0 and similarly we set the start_boxe variable using Electro-Pneumatic logic.

PLC_EPN2: This is a boolean variable. Once we switch on the PLC-EPN toggle switch (created in HMI) then this PLC_EPN2 will be set to 1 otherwise it will remain 0.

start_sensep and start_sensee: Both are boolean variables. Once we switch on the Sensors toggle switch (created in HMI) then start_sensep will be set to 1 using PLC logic otherwise start_sensep will be set to 0 and similarly we set the start_sensee variable using Electro-Pneumatic logic.

start_convp and start_conve: Both are the boolean variables. Once we switch on the Start Conveyor toggle switch (created in HMI) then start_convp will be set to 1 using PLC logic otherwise start_convp will be set to 0 and similarly we set the start_conve variable using Electro-Pneumatic logic.

stopp and stope: This is a boolean variable. Once we press the Stop button (created in HMI) then stopp will be set to 1 using PLC logic otherwise stopp will be set to 0 and similarly we set the stope variable using Electro-Pneumatic logic.

1Y1, 1Y2, 1Y3 and 1Y4: These are the boolean variables that require the Sensor1 and Sensor2 signals; those are the output variables in SFC and work as the input variable for the PLC circuit (1Y1 and 1Y2) and Electro-Pneumatic circuit (1Y3 and 1Y4). Once both sensors are set to 1 then we energize a bit and set 1Y1 to 1 if sensor2 is on and bit is energized else if sensor2 is on but the bit is not energized, then we set 1Y2 to 1 using PLC logic circuit and similarly we set the 1Y3 and 1Y4 using the Electro-Pneumatic logic circuit.

#### Output variables

start_box: It’s a boolean variable and set to one if start_boxe or start_boxp is 1.

start_sense: It’s a boolean variable and set to one if start_sensee or start_sensep is 1 and will start the sensors.

start_conv: It’s a boolean variable and set to one if start_conve or start_convp is 1 and will start the conveyors.

stop: It’s a boolean variable and set to one if stope or stopp is 1 and will stop the Conveyors and as well as boxes will also be stopped.

E_stop: It’s a boolean variable and set to one once the Emergency stop button (created in HMI) is pressed. Once the E_stop button is pressed then it will stop everything in the virtual system. After pressing the E_stop button start_box, start_sense and start_conv will be set to zero.

reset: It’s a boolean variable and set to one once the reset button (created in HMI) is pressed and will reset everything means it will set every variable zero or their default value. This button we created for our debug purpose.

Type: It’s a boolean variable and linked to the Type1-Type2 toggle switch. If the toggle switch is set on Type1 then the Type variable is set to one else it is set to zero.

length: It’s a real variable and set to 40 if the Type variable is set to one and set to 20 if the Type variable is set to zero.

box: It’s a real value variable and linked with the box (Type2 and length = 20) (created in the virtual system). This will start increasing once start_conv, start_box and start_sense is on. First it will increase by 1 x HMI16_1."speed" till 160 (movement on first conveyor) . After that, first rotate the box and then start increasing the box by 1.5 till 220 and then after rotating again opposite it increase by 1 x HMI16_1."speed" till 310. Once the box variable value exceeds 310 then it will set to be zero. This variable corresponds to the horizontal motion of the box.

box_r: It’s a real value variable and linked with the box (Type2 and length = 20) (created in the virtual system). Once box value is reached the 160 then box_r variable is increased by 1 till 16 and box starts rotating in clockwise direction. Once box value is reached the 220 then box_r decreases by 1 till 0 and box start_rotating in anticlockwise direction. This variable corresponds to the rotation of the box in clockwise and anticlockwise direction.

box_y: It’s a real value variable and linked with the box (Type2 and length = 20) (created in the virtual system). This will start increasing by 2.6 once box value is reached 160 and box is rotated and will keep increasing till box value is reached 220. This variable corresponds to the vertical motion of our box.

box1, box1_r and box1_y: It’s a real value variable and this variable is linked with Type1 box (length = 40). These variables will also increase in similar fashion like box box_r and box_y respectively, only value may change according to box length.

cyl: It’s a real value variable and this variable is linked with a cylinder. If the signal comes from 1Y2 or 1Y4 then the cyl variable is decreased by 6 till 0. If the signal comes from 1Y1 or 1Y3 then the cyl variable is increased by 2 till 45. Because we want the Type1 boxes to go on the upper conveyor and Typ2 boxes to go on the lower conveyor so if type1 boxes are coming then the cylinder will extend and if type2 boxes are coming the cylinder will retract.

plate: It’s a real value variable and this variable is linked with a plate on which boxes are moving and transferring from one conveyor to another. This variable is for rotating the plate with the cyl value. If the cylinder extends then the plate will rotate anticlockwise or if the cylinder retracts then the plate will rotate clockwise.

sensor1: It’s a boolean variable and will remain 1 for duration when the box is below the sensor 1 that we adjusted with the box value and once the box is moved further and not below the sensor 1 it will again set 0.

sensor2: It’s a boolean variable and will remain 1 for duration when the box is below the sensor 2 that we adjusted with the box value and once the box is moved further and not below the sensor 2 it will again set 0.

![Screenshot 2022-11-17 231708](https://user-images.githubusercontent.com/59050030/202519833-0a6b5a33-9c7a-43ca-8308-d0f1bb3b2acf.jpg)

Figue 2: Virtual Representation of the desired automation

## 5. Conclusion

In addition to the successful completion of the project's deliverable box distribution, the criterion for a continuous box supply has also been met. For example, we used a selector switch to change the type of incoming box. This would necessitate the usage of external scripts. Accelerations enable the implementation of features such as gravity feed, and velocity inputs can be used to enhance the movement of the boxes. The entire automation can be linked to any other form of automation, just as was done in prior projects for this class, so that different factory components can communicate. For example, after punching, a component of our project may transfer the boxes from one spot to the conveyor, which would then use the length of the boxes to separate the boxes into different piles. The instance is likewise capable of operating in the opposite direction. In conclusion, the software can be designed so that it is functional not just for the specified lengths, but also for any other lengths, so long as the sensors are functioning over the appropriate time period.

## 6. Learnings

The project helped us in learning Automation Studio software in detail and also polished some concepts of PLC and EPN as we practiced them and virtually solved the problem assigned efficiently. As part of this project, we learnt SFC coding, making Virtual systems, making PLC ladder logic and EPN power and control circuit of an industrial level requirement. We also learnt how to make HMI with suggestions from TAs like it should be such that the user does not have to use manuals again and again and the indicator lights are quite helpful for this along with speed adjusting slider and stop as well as emergency stop button. We got to know where all emergency stop buttons are connected in logic circuits and also the importance of power circuit in EPN and different voltage assignment in power circuit than control circuit. PLC ladder logic and EPN can be used alternatively in our mechanism using a single selector switch and while making this, we got a deeper understanding of Virtual system variables and how SFC code works. Overall, we got a good understanding of software and coding and also making the logic work in coordination with virtual systems.

