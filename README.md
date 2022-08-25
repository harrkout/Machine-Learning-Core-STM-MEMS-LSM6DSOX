## <center>Machine Learning Core with STM MEMS Sensor LSM6DSOX

<br>

<center>This report demonstrates how to use the LSM6DSOX Machine Learning Sensor in combination with the B-L072Z-LRWAN1 and the X-NUCLEO-IKS01A3, create a Decision Tree with Weka and use it to get feedback from the actions performed.</center>

<br>

---

#### <center> Hardware that was used in this project:

| <center>Board</center> | <center> Extender</center> | <center>Adapter</center> |
| --- | --- | --- |
| <center>B-L072Z-LRWAN1</center> | <center>X-NUCLEO-IKS01A3</center> | <center>MKI197V1 (LSM6DSOX)</center> |

</center>
<br>

---

- *Getting the Firmware and Software needed*:

The **Firmware** that was used is the [X-CUBE-MEMS1 Firmware Package.](http://github.com/STMicroelectronics/X-CUBE-MEMS1/tree/main/Projects/NUCLEO-L073RZ/Examples/IKS01A3/DataLogExtended/STM32CubeIDE)

**Note**: Inside the repository folder **Projects**, the only available options are Nucleo-based projects. The ones I tested though, work just fine with other ST boards, since we are using one of the **Extenders** that are compatible: (**IKS01A1/IKS01A2/IKS01A3**).

The project linked above needs to be programmed and flashed with the STM32CubeIDE program, since it does not include a CubeMX project.

---

The **Software** used is:

1. **Unicleo-GUI**: Demonstrates the functionality of ST sensors and algorithms
2. **Unico**: Needed to create the .ucf file that will be loaded after the Decision Tree is created.
3. **Weka**: Machine Learning Tool used for Data Mining tasks.

---

<br>

#### <center>**Hardware Combination and Specifications.**

<br>

1. **B-L072Z-LRWAN1**

![1](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/1_1.JPG?msec=1661410385356)

<br>

2. **X-NUCLEO-IKS01A3**

![2](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/2_2.JPG?msec=1661410385353)

<br>

3. **MKI197V1 (LSM6DSOX)**

![3](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/3_3.JPG?msec=1661410385316)<br>

<br>

- **First we need to combine the X-NUCLEO-IKS01A3 on the Arduino R3 pinouts of B-L072Z-LRWAN1**

<br>

![4](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/4_4.jpg?msec=1661410385414)

<br>

- **Next we add the MKI197V1 on the DIL24 Socket of the IKS01A3**

> **IMPORTANT**: Make sure the ST Logo on BOTH the extender and the adapter are aligned, otherwise the adapter will overheat and most likely circuit itself. ![5](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/5_5.JPG?msec=1661410385359) 

<br>

- **Now for the final step**, we need to enable the LSM6DSOX sensor on the MKI197V1. The LSM6DSOX sensor starts in I3C mode because of a level shifter on the IKS01A3 that keeps the INT1 of the LSM6DSOX high, this results to I3C initialization by default (as described in the Datasheet). The only solution that I found was to bypass the INT1 and route the INT2 in its place. That can be done by connecting the A5 pin of the IKS01A3 to GND with a wire and also change the JP6 Jumper from the default 5-6 to 13-14. The change of the Jumper supposedly sets the M_INT2_0 on pin D2, in case a change need to be made in the schematic.
   After these changes, the LSM6DSOX should be enabled.

<br>

![6](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/6_6.JPG?msec=1661410385380)

<br>

#### <center> **Now we are ready to begin.**

---

<br>

- First we need to program and flash our board. This needs to be done with STM32CubeIDE.

![7](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/ide.png?msec=1661410385233)

<br>

- Now let's check if our sensor works as intented in Unicleo.
  
  - > This is the interface of Unicleo. If our board is connected via USB (ST Link) then the Serial Port should be automatically selected, mine for example is COM5. *We select* `Connect`.
    
    ![71](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unicleo1.png?msec=1661410385298)
    

<br>

- The sensors list will pop up and show all available sensors. Choose the LSM6DSOX (DIL24).
  
  - > Note that there is also another sensor named LSM6DSO, that is the exaxt same sensor but on the extender IKS01A3 and does not contain the Machine Learning Core.
    
    ![72](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unicleo1_2.png?msec=1661410385295)
    

<br>

- After `Apply` has been selected, the following window will pop up. It shows the sensors of the IKS01A3 and the MKI197V1 along with their locations.
  
  - > On the left we can see the option for the visualization of the available sensors.
    
    ![8](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unicleo2.png?msec=1661410385294)
    

<br>

- If `MLC` is chosen on the left, we will be greeted by the following window. On the first block, named `Sensor Configuration`, a custom .ucf dataset can be loaded. Below that are some `Example algorithms` that if chosen they will be loaded and according to the motion of the sensor, a different value will be shown on the `MLC Source Registers` bellow.
  
  - > The values will be displayed on the blocks in the right of `MLC0_SRC`.
    
    <br>
    <img src="unicleo3.png" title="" alt="9" data-align="center">
    

<br>

- For example, here are the specifics of the `Activity Recognition (Wrist)` algorithm.
  
  - > According to the documentation
    
    - 1 = Stationary/Other
      
    - 4 = Walking/FastWalking
      
    - 8 = Jogging/Running
      
      <br>
      <img src="unicleo4.png" title="" alt="10" data-align="center">
      

<br>

- Before we can select the algorithm we need to select `Start` on the main window. That way we initialize the sensors to begin monitoring.

<br>

<br>
  <img src="unicleo5.png" title="" alt="11" data-align="center">

<br>

- Now, instead of using one of the example algorithms, I will create my own `.ucf` file. To do that, I need to log data based on each action that I want to add to the Decision Tree. The log will be created in Unicleo (in `.txt/.csv` format) and then I'll use Unico to create the `.ucf` file. Then I'll load the `.ucf` file to Weka to create the Decision Tree and load it back to Unicleo.
  
  <br>
  
- In order to log the data, the `Datalog` option needs to be chosen on the left on the main window. Next choose the sensors that you want to log into a file from both `Data` and `Datalog period source` and set a file for said activity. Here I will monitor some actions for 1 minute and save them in a file `karate.csv`.
  
  <br>
  

![12](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unicleo6.png?msec=1661410385207)

<br>

- I will follow the same steps for the dataset `boxing.csv`.
  
  <br>
  <img src="unicleo7.png" title="" alt="13" data-align="center">
  

<br>

<br>

- Now *Unico* needs to be used in order to create a file that can be read by Weka.
  
  - > Since Unico cannot be used with the shield IKS01A3, I will have to use it in offline mode. That means that it will not connect with my board, but I will be able to load my datasets in order to extract an `.arff` file.
    
  - > > Select in the `iNemo Inertial Modules` the `STEVAL-MKI197V1 (LSM6DSOX)` sensor and diselect the `Communication with the motherboard` option. Then click on `Select Device`.
    
    <br>
    <img src="unico1.png" title="" alt="14" data-align="center">
    

<br>

<br>

- After the main window shows up, select `MLC` on the left and the Machine Learning Core window will open.

![141](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico2.png?msec=1661410385278)

<br>

- Now we load each one of the datasets and set the `Class (Label)` as `boxing` for the boxing dataset and `karate` for the karate dataset.

![15](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico3.png?msec=1661410385209)

<br>

- Then we move to the `Configuration` Tab. Here are the options that I gave to my Decision Tree.

![16](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico4.png?msec=1661410385209)

<br>

![17](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico5.png?msec=1661410385208)

<br>

![18](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico6.png?msec=1661410385209)

<br>

- I chose all the *Signed* options `ACC_X`, `ACC_Y`, `ACC_Z`, `GY_X`, `GY_Y`, `GY_Z` for the `Mean`, `Variance`, `Energy` and `Peak to Peak` features.

<br>

![19](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico7.png?msec=1661410385210)

<br>

- Here I name a file to save as `.arff` and choose the output that I want to see in the Decision Tree later.
  - > I set `1` for karate and `2` for boxing.
    

![19](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico8.png?msec=1661410385210)

<br>
<br>

<br>

- Open Weka Explorer and load the `sports.arff` file we created above:

![20](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/weka1.png?msec=1661410385211)

<br>

- Select the `Clasify Tab` and then `Choose` on the `Classifier` Block and select the `J48` in the tree section. I let the `Cross-Validation Folds` on 10 as default since it gives a ~98% result.
  
  <br>
  

<img src="weka2.png" title="" alt="21" data-align="center">
  <br>
  <br>

<br>

![22](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/weka3.png?msec=1661410385211)

<br>

- The Decision Tree has be generated. But in order to load it into Unico to create our `.ucf` file we need to copy the selected text as in the image below (The tree itself) and paste it into a `.txt` file.

<br>

![23](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/weka4.png?msec=1661410385236)

<br>

![24](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/weka5.png?msec=1661410385212)

- And if we want to see the Decision Tree, then right click on the `Result list` and select `Visualize Tree`.

<br>

![25](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/weka6.png?msec=1661410385212)

<br>

- Now we can load the `.txt` file into the Unico window we left intact a while ago.

![26](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/weka7.png?msec=1661410385212)

- And save the file as `sports.ucf`.

![27](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/unico9.png?msec=1661410385213)

<br>

- Now we can go back to Unicleo and load the `sports.ucf` file to the `MLC`.
  
  ![28](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/final1.png?msec=1661410385237)
  
  - > Here as we can see the `MLC0_SRC` changes to `1` when the board mimics the action for `karate`.
    
  
  ![29](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/final2.png?msec=1661410385238)
  
  - > And here as we can see the `MLC0_SRC` changes to `2` when the board mimics the action for `boxing`.
    
  
  ![30](file:///home/harrkout/Documents/ISCA/Machine Learning/MLC Report/final3.png?msec=1661410385237)
  

---

- Here is a video demostration of the LSM6DSOX sensor changing value from `1` to `2` when it recognises the different actions.
  
  > [ Machine Learning Core LSM6DSOX Demonstration || ISCA Lab](https://www.youtube.com/watch?v=m6ylfVGBezo)
  

---

##### <center> This concludes this demo.

---

- Some notes on the `Machine Learning Core` and the `Finite State Machine` options on Unicleo/Unico.
  
  - > `Finite State Machine` gives only a True or False result, from my understanding. This means that it can only detect whether the sensor is Idle or in a specific Action, it cannot distinguish between different events and actions.
    
  - > Whereas the Machine Learning Core **can** distinguish between different actions performed by the sensor. For example it can tell us when the sensor is Idle or when a specific action (from numerous loaded in the Decision Tree) is performed.
