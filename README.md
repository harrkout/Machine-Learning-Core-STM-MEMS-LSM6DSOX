## <center>Machine Learning Core with STM MEMS Sensor LSM6DSOX

<br>

<center>This report demonstrates how to use the LSM6DSOX Machine Learning Sensor in combination with the B-L072Z-LRWAN1 and the X-NUCLEO-IKS01A3, create a Decision Tree with Weka and use it to get feedback from the actions performed.</center>

<br>

---

#### <center> Hardware that was used in this project:

| <center>Board</center>          | <center> Extender</center>        | <center>Adapter</center>             |
| ------------------------------- | --------------------------------- | ------------------------------------ |
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

<center>1. B-L072Z-LRWAN1</center>

<img title="" src="1_1.JPG" alt="1" data-align="center" width="553">

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<br>

<br>

<br>

<center>2. X-NUCLEO-IKS01A3</center>

<br>

<img title="" src="2_2.JPG" alt="2" data-align="center" width="551">

<br>

<br>

<center>3. MKI197V1 (LSM6DSOX)</center>

<br>

<img title="" src="3_3.JPG" alt="3" data-align="center" width="554"><br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

- **First we need to combine the X-NUCLEO-IKS01A3 on the Arduino R3 pinouts of B-L072Z-LRWAN1**  

<br>

<img title="" src="4_4.jpg" alt="4" data-align="center" width="573">

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

- **Next we add the MKI197V1 on the DIL24 Socket of the IKS01A3**

> **IMPORTANT**: Make sure the ST Logo on BOTH the extender and the adapter are aligned, otherwise the adapter will overheat and most likely circuit itself.        <img title="" src="5_5.JPG" alt="5" data-align="center" width="513">

<br>

- **Now for the final step**, we need to enable the LSM6DSOX sensor on the MKI197V1. The LSM6DSOX sensor starts in I3C mode because of a level shifter on the IKS01A3 that keeps the INT1 of the LSM6DSOX high, this results to I3C initialization by default (as described in the Datasheet). The only solution that I found was to bypass the INT1 and route the INT2 in its place. That can be done by connecting the A5 pin of the IKS01A3 to GND with a wire and also change the JP6 Jumper from the default 5-6 to 13-14. The change of the Jumper supposedly sets the M_INT2_0 on pin D2, in case a change need to be made in the schematic.
   After these changes, the LSM6DSOX should be enabled.

<br>

<img title="" src="6_6.JPG" alt="6" data-align="center" width="570">

<br>

#### <center> **Now we are ready to begin.**

---

<br>

- First we need to program and flash our board. This needs to be done with STM32CubeIDE. 

<img title="" src="ide.png" alt="7" data-align="center" width="741">

<br>

- Now let's check if our sensor works as intented in Unicleo.
  
  - > This is the interface of Unicleo. If our board is connected via USB (ST Link) then the Serial Port should be automatically selected, mine for example is COM5. *We select* `Connect`.
    
    <img title="" src="unicleo1.png" alt="71" data-align="center" width="717">

<br>

- The sensors list will pop up and show all available sensors. Choose the LSM6DSOX (DIL24).
  
  - > Note that there is also another sensor named LSM6DSO, that is the exaxt same sensor but on the extender IKS01A3 and does not contain the Machine Learning Core. 
    
    <img title="" src="unicleo1_2.png" alt="72" data-align="center" width="656">

<br>

- After `Apply` has been selected, the following window will pop up. It shows the sensors of the IKS01A3 and the MKI197V1 along with their locations.
  
  - > On the left we can see the option for the visualization of the available sensors. 
    
    <img title="" src="unicleo2.png" alt="8" data-align="center" width="620">

<br>

- If `MLC` is chosen on the left, we will be greeted by the following window. On the first block, named `Sensor Configuration`, a custom .ucf dataset can be loaded. Below that are some `Example algorithms` that if chosen they will be loaded and according to the motion of the sensor, a different value will be shown on the `MLC Source Registers` bellow.
  
  - > The values will be displayed on the blocks in the right of `MLC0_SRC`.
    
    <img title="" src="unicleo3.png" alt="9" data-align="center" width="327">

<br>

- For example, here are the specifics of the `Activity Recognition (Wrist)` algorithm.
  
  - > According to the documentation
    
    - 1 = Stationary/Other
    
    - 4 = Walking/FastWalking
    
    - 8 = Jogging/Running
      
      <img src="unicleo4.png" title="" alt="10" data-align="center">
      
      <br>

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

<img title="" src="unicleo6.png" alt="12" data-align="center">

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

<img src="unico2.png" title="" alt="141" data-align="center">

<br>

<br><br>

- Now we load each one of the datasets and set the `Class (Label)` as `boxing` for the boxing dataset and `karate` for the karate dataset. 

<img src="unico3.png" title="" alt="15" data-align="center">

<br>

<br>

<br>

<br>

- Then we move to the `Configuration` Tab. Here are the options that I gave to my Decision Tree.

<img src="unico4.png" title="" alt="16" data-align="center">

<br>

<img src="unico5.png" title="" alt="17" data-align="center">

<br>

<img src="unico6.png" title="" alt="18" data-align="center">

<br>

<br><br><br>

- I chose all the *Signed* options `ACC_X`, `ACC_Y`, `ACC_Z`, `GY_X`, `GY_Y`, `GY_Z` for the `Mean`, `Variance`,  `Energy` and `Peak to Peak` features.

<br>

<img src="unico7.png" title="" alt="19" data-align="center">

<br>

<br><br>

<br>

- Here I name a file to save as `.arff` and choose the output that I want to see in the Decision Tree later. 
  
  - > I set `1` for karate and `2` for boxing.

<img src="unico8.png" title="" alt="19" data-align="center">

<br>
<br>

<br>

<br><br>

<br>

- Open Weka Explorer and load the `sports.arff` file we created above:

<img src="weka1.png" title="" alt="20" data-align="center">

<br>

<br><br><br>

- Select the `Clasify Tab` and then `Choose` on the `Classifier` Block and select the `J48` in the tree section. I let the `Cross-Validation Folds` on 10 as default since it gives a ~98% result. 

<br>

<img src="weka2.png" title="" alt="21" data-align="center">
  <br>
  <br>

<br>

<img title="" src="weka3.png" alt="22" data-align="center" width="250">

<br>

<br><br>

<br>

- The Decision Tree has be generated. But in order to load it into Unico to create our `.ucf` file we need to copy the selected text as in the image below (The tree itself) and paste it into a `.txt` file. 

<br>

<img title="" src="weka4.png" alt="23" data-align="center" width="973">

<br>

<img src="weka5.png" title="" alt="24" data-align="center">

<br><br>

<br>

- And if we want to see the Decision Tree, then right click on the `Result list` and select `Visualize Tree`.

<br>

<img title="" src="weka6.png" alt="25" data-align="center" width="465">

<br>

<br>

<br>

<br>

- Now we can load the `.txt` file into the Unico window we left intact a while ago. 

<img title="" src="weka7.png" alt="26" data-align="center" width="605">

<br>

<br>

<br>

- And save the file as `sports.ucf`.

<img title="" src="unico9.png" alt="27" data-align="center" width="629">

<br>

<br><br>

<br>

- Now we can go back to Unicleo and load the `sports.ucf` file to the `MLC`. 

<img title="" src="final1.png" alt="28" data-align="center" width="392">

<br>

<br>

<br>

- > Here as we can see the `MLC0_SRC` changes to `1` when the board mimics the action for `karate`.

<img title="" src="final2.png" alt="29" data-align="center" width="410">

<br>

<br>

<br>

- And here as we can see the `MLC0_SRC` changes to `2` when the board mimics the action for `boxing`.

<img title="" src="final3.png" alt="30" data-align="center" width="412">

---

- <u>**Here is a video demostration of the LSM6DSOX sensor changing value from `1` to `2` when it recognises the different actions**</u>.
  
  <br>
  
  > 
  > 
  > [<center> Machine Learning Core LSM6DSOX Demonstration || ISCA Lab</center>](https://www.youtube.com/watch?v=m6ylfVGBezo)
  > 
  > 
  > <br>
  > 
  > 
  > [![test](https://raw.githubusercontent.com/harrkout/Machine-Learning-Core-LSM6DSO-X/main/6_6.JPG)](https://www.youtube.com/watch?v=m6ylfVGBezo)

##### <center> This concludes this demo.

---

- Some notes on the `Machine Learning Core` and the `Finite State Machine` options on Unicleo/Unico.
  
  - > `Finite State Machine` gives only a True or False result, from my understanding. This means that it can only detect whether the sensor is Idle or in a specific Action, it cannot distinguish between different events and actions.
  
  - > Whereas the Machine Learning Core **can** distinguish between different actions performed by the sensor. For example it can tell us when the sensor is Idle or when a specific action (from numerous loaded in the Decision Tree) is performed.
