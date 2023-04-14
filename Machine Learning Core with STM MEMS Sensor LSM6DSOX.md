
# Machine Learning Core with STM MEMS Sensor LSM6DSOX

­­

**Machine Learning Core with STM MEMS**

**Sensor LSM6DSOX**

Author: Harry Koutsourelakis

V9.0

14 Sep 2022

**Abstract**

This work presents the methodology to develop AI-based applications for Internet-of-Things environments, such as detection of a falling object by using the machine learning core inside the STM Mems Sensor LSM6DSOX and the toolchain Unico, Unicleo and WEKA.

# Table of Contents

[**Abstract**](#_Toc119493230)

[1 **Equipment** 4](#_Toc119493231)

[1.1 **B-L072Z-LRWAN1** 4](#_Toc119493232)

[1.2 **X-NUCLEO-IKS01A3** 5](#_Toc119493233)

[1.3 **STEVAL-MKI197V1** 6](#_Toc119493234)

[2 **Hardware Synthesis** 7](#_Toc119493235)

[2.1 **Connection between B-L072Z-LRWAN1 and X-NUCLEO-IKS01A3** 7](#_Toc119493236)

[2.2 **Connection between X-NUCLEO-IKS01A3 and STEVAL-MKI197V1** 8](#_Toc119493237)

[2.3 **LSM6DSOX Sensor Initialization** 8](#_Toc119493238)

[3 **Firmware and Software** 9](#_Toc119493239)

[3.1 **Firmware** 9](#_Toc119493240)

[3.2 **Unicleo-GUI** 10](#_Toc119493241)

[3.3 **Unico-GUI** 10](#_Toc119493242)

[3.4 **Weka** 11](#_Toc119493243)

[4 **Implementation** ](#_Toc119493244)

[4.1 **STM32CubeIDE** 12](#_Toc119493245)

[4.2 **Unicleo-GUI** 12](#_Toc119493246)

[4.3 **Machine Learning Core Example** 15](#_Toc119493247)

[4.4 **Unico-GUI** 18](#_Toc119493248)

[4.5 **Weka** 21](#_Toc119493249)

[4.6 **Video Demonstration** 27](#_Toc119493250)

[5 **Falling**** Detection ****Algorithm** 28](#_Toc119493251)

[5.1 **Creation of Falling Detection Datasets** 28](#_Toc119493252)

[5.2 **Project Equipment** 29](#_Toc119493253)

[5.3 **Video Demonstration** 29](#_Toc119493254)

[5.4 **Possible Real-World Use Scenarios** 30](#_Toc119493255)

[6 **Falling Detection Algorithm With NUCLEO-WL55JCx and SHUBv3** 31](#_Toc119493256)

[6.1 **Equipment** 32](#_Toc119493257)

[6.2 **Decision Tree Generation** 34](#_Toc119493258)

[6.2.1 **K-fold Cross-Validation** 34](#_Toc119493259)

[6.2.2**J48 Algorithm (also known as C4.5)** 34](#_Toc119493260)

[6.3 **Performance** 36](#_Toc119493261)

[6.4 **Video Demonstration** 36](#_Toc119493262)

[7 **References** 37](#_Toc119493263)

# 1 **Equipment**

The equipment used in this project consists of the board [B-L072Z-LRWAN1](https://www.st.com/en/evaluation-tools/b-l072z-lrwan1.html), the [X-NUCLEO-IKS01A3 extender and the STEVAL-MKI197V1. The IKS01A3 is the extension board that provides the DIL24 socket that the MKI197V1 (Machine Learning Core LSM6DOX) connects to in order to communicate with the board.](https://www.st.com/en/ecosystems/x-nucleo-iks01a3.html)

## 1.1 **B-L072Z-LRWAN1**

The **B-L072Z-LRWAN1** [[1]](#_References) board was chosen specifically for this project due to its portability, LoRaWan Connectivity and low battery consumption.

![Shape1](RackMultipart20230414-1-bpee3o_html_cbf09e657ca6919c.gif)

Specifications

![Shape2](RackMultipart20230414-1-bpee3o_html_9a57e65a74cd3efb.gif)
- CMWX1ZZABZ-091 LoRa®/Sigfox™ module (Murata)
  - Embedded ultra-low-power STM32L072CZ MCU, based on Arm® Cortex®-M0+ core, with 192 Kbytes of Flash memory, 20 Kbytes of RAM, 20 Kbytes of EEPROM
  - Frequency range: 860 MHz - 930 MHz
  - USB 2.0 FS
  - 4-channel,12-bit ADC, 2 × DAC
  - 6-bit timers, LP-UART, I2C and SPI
  - Embedded SX1276 transceiver
  - LoRa®, FSK, GFSK, MSK, GMSK, and OOK modulations (+ Sigfox™ compatibility)
  - Programmable bit rate up to 300 kbit/s
  - High sensitivity: down to -137 dBm
  - Bullet-proof front end: IIP3 = -12.5 dBm
  - 89 dB blocking immunity
  - Low Rx current of 10 mA, 200 nA register retention
  - Fully integrated synthesizer with a resolution of 61 Hz
  - Built-in bit synchronizer for clock recovery
  - Sync word recognition
  - Preamble detection
  - LoRaWAN™ Class A certified
- SMA and U.FL RF interface connectors
- Including 50-ohm SMA RF antenna
- 7 LEDs:
  - 4 general-purpose LEDs
  - 5 V power LED
  - ST-LINK-communication LED
  - Fault-power LED
- 1 user and 1 reset push-buttons
- On-board ST-LINK/V2-1 debugger/programmer with USB re-enumeration capability: mass storage, Virtual COM port, and debug port
- Arduino™ Uno V3 connectors
- Board power supply through the USB bus or external VIN/3.3 V supply voltage or batteries
- 3 × AAA-type battery holder for standalone operation
- Support of a wide choice of Integrated Development Environments (IDEs) including IAR™, Keil®, GCC-based IDEs, Arm® Mbed™

[**B**](https://www.st.com/en/evaluation-tools/b-l072z-lrwan1.html) ![](RackMultipart20230414-1-bpee3o_html_ff92ce00dadd1503.jpg)
[-L072Z-LRWAN1](https://www.st.com/en/evaluation-tools/b-l072z-lrwan1.html)

![Picture 240](RackMultipart20230414-1-bpee3o_html_acccf2251549b0a0.gif)

## 1.2 **X-NUCLEO-IKS01A3**

The [X-NUCLEO-IKS01A3](https://www.st.com/en/ecosystems/x-nucleo-iks01a3.html)[[2]](#_References)is a is a motion MEMS and environmental sensor evaluation board system. It is compatible with the Arduino UNO R3 connector layout and features the LSM6DSO 3-axis accelerometer + 3-axis gyroscope, the LIS2MDL 3-axis magnetometer, the LIS2DW12 3-axis accelerometer, the HTS221 humidity and temperature sensor, the LPS22HH pressure sensor, and the STTS751 temperature sensor.

The X-NUCLEO-IKS01A3 interfaces with the STM32 microcontroller via the I²C pin, and it is possible to change the default I²C port.

![Shape3](RackMultipart20230414-1-bpee3o_html_1987979f4ad7d1b0.gif) ![Shape4](RackMultipart20230414-1-bpee3o_html_eda9612467a44253.gif)

- LSM6DSO: MEMS 3D accelerometer (±2/±4/±8/±16 g) + 3D gyroscope (±125/±250/±500/±1000/±2000 dps)
- LIS2MDL: MEMS 3D magnetometer (±50 gauss)
- LIS2DW12: MEMS 3D accelerometer (±2/±4/±8/±16 g)
- LPS22HH: MEMS pressure sensor, 260-1260 hPa absolute digital output barometer
- HTS221: capacitive digital relative humidity and temperature
- STTS751: Temperature sensor (–40 °C to +125 °C)
- DIL 24-pin socket available for additional MEMS adapters and other sensors

- Free comprehensive development firmware library and example for all sensors compatible with STM32Cube firmware
- I²C sensor hub features on LSM6DSO available
- Compatible with STM32 Nucleo boards
- Equipped with Arduino UNO R3 connector
- RoHS compliant
- WEEE compliant

Specifications


[ **X-NUCLEO-IKS01A3**
](https://www.st.com/en/ecosystems/x-nucleo-iks01a3.html)

![](RackMultipart20230414-1-bpee3o_html_c820c66e75c30e63.jpg)

## 1.3 **STEVAL-MKI197V1**

The [STEVAL-MKI197V1](https://www.st.com/en/evaluation-tools/steval-mki197v1.html)[[3]](#_References)is an adapter board designed to facilitate the evaluation of MEMS devices in the LSM6DSO product family. The board offers an effective solution for fast system prototyping and device evaluation directly within the user's own application.

![Shape6](RackMultipart20230414-1-bpee3o_html_63f12b188951f79b.gif) ![Shape5](RackMultipart20230414-1-bpee3o_html_4f9832c05d945c2c.gif)

Specifications

- Complete LSM6DSOX pinout for a standard DIL 24 socket
- Fully compatible with STEVAL-MKI109V3 motherboard
- Changing the resistor settings is also compatible with the STEVAL-MKI109V2 motherboard
- WEEE compliant
- RoHS compliant


 STEVAL-MKI197V1

![](RackMultipart20230414-1-bpee3o_html_29007e903655a0d3.jpg)

# 2 **Hardware Synthesis**

## 2.1 **Connection between**  **B-L072Z-LRWAN1 and**** X-NUCLEO-IKS01A3**

First,the **X-NUCLEO-IKS01A3** needs to be connected totheArduinoR3 pinoutsof **B-L072Z-LRWAN1**. This gives the option to connect the **STEVAL-MKI197V1** to the DIL24 socket on top of the Nucleo extender.

![](RackMultipart20230414-1-bpee3o_html_a8f7edb458e6c93e.jpg)

## 2.2 **Connection between**  **X-NUCLEO-IKS01A3 and**  **STEVAL-MKI197V1**

Next, the **STEVAL-MKI197V1** must be connected on the **DIL24 Socket** of the **X-NUCLEO-IKS01A3**.

**IMPORTANT** :MakesuretheSTLogoonBOTHtheextenderandtheadapterarealigned,otherwisetheadapterwilloverheatandmost likely lead to short-circuit.

![](RackMultipart20230414-1-bpee3o_html_d965206a62c8761a.jpg)

## 2.3 ![Shape8](RackMultipart20230414-1-bpee3o_html_c6a4cdcc9ba1e860.gif) **LSM6DSOX Sensor Initialization**

The **LSM6DSOX** (Machine Learning Core) sensor on the **MKI197V1** does not communicate out of the box with the **IKS01A3**. The **LSM6DSOX** sensor starts in **I3C** [[4]](#_References) mode (also known as SenseWire) because of a level shifter on the **IKS01A3** that keeps the INT1 of the **LSM6DSOX** high, this results to **I3C** initialization by default (as described in theDatasheet).

The only solution that was found was to bypass the INT1 and route the INT2 in its place. That can be done by connecting the A5 pin of the IKS01A3 to GND with a wire and also change the JP6 Jumper from the default 5-6 to 13-14. The change of the Jumper supposedly setsthe M\_INT2\_0 on pin D2, in case a change needs to be made in the schematic.

**After these changes, the LSM6DSOX sensor will be enabled.**

![](RackMultipart20230414-1-bpee3o_html_f31b2af7cd2932cc.jpg)

# 3 **Firmware and Software**

## 3.1 **Firmware**

The Firmware that was used is [the X-CUBE-MEMS1 Firmware Package](https://github.com/STMicroelectronics/X-CUBE-MEMS1)[[5]](#_References).

The X-CUBE-MEMS1 expansion software package for STM32Cube runs on the STM32 and includes drivers that recognize the sensors and collect temperature, humidity, pressure, and motion data. The expansion is built on STM32Cube software technology to ease portability across different STM32 microcontrollers. The software comes with a sample implementation of the drivers running on the X-NUCLEO-IKS01A2/X-NUCLEO-IKS01A3/X-NUCLEO-IKS02A1 expansion boards connected to a featured STM32 Nucleo development board. The software is also available on GitHub, where the users can signal bugs and propose new ideas through Issues and Pull requests tabs.

**Note** :Insidetherepositoryfolder **Projects** ,theonlyavailableoptionsareNucleo-basedprojects.Theonesthat were testedthough,workjustfinewith otherST boards, since one of the **Extenders** that are compatible ( **IKS01A1/IKS01A2/IKS01A3** ), are used.

## 3.2[**Unicleo-GUI**](https://www.st.com/en/development-tools/unicleo-gui.html)

**Unicleo-GUI** [[6]](#_References) is a graphical user interface (GUI) for the X-CUBE-MEMS1 and X-CUBE-MEMS-XT1 software expansions and STM32Nucleo expansion boards (X-NUCLEO-IKS01A1, X-NUCLEO-IKS01A2, X-NUCLEO-IKS01A3 and X-NUCLEO-IKS02A1). The main objective of this application is to demonstrate the functionality of ST sensors and algorithms.

Unicleo-GUI is able to cooperate with firmware created by AlgoBuilder application and display data coming from the running firmware.

The application is also able to establish Bluetooth connection with BLE connectivity-equipped devices such as SensorTile (STEVAL-STLKT01V1), BlueCoin (STEVAL-BCNKT01V1), and STM32 Nucleo with X-NUCLEO-IDB05A1 expansion board, BlueTile(STEVAL-BCN002V1B)orWESU1(STEVAL-WESU1)andreaddatafromvariousdevicecharacteristics.Thesupportedfirmware forthese devices can be found at FP-SNS-ALLMEMS1, FP-SNS-ALLMEMS2, FP-SNS-MOTENV1, FP-SNS-MOTENVWB1, STSW-BLUETILE-DKand STSW-WESU1.

## 3.3[**Unico-GUI**](https://www.st.com/en/development-tools/unico-gui.html)

**Unico-GUI** [[7]](#_References)isacomprehensivesoftwarepackagefortheevaluationboardsofallMEMSsensorsavailableinST'sproductportfolio(accelerometers, gyroscopes, magnetometers and environmental sensors).

The softwareisacross-platformgraphicaluserinterfaceinteractingwithSTEVAL-MKI109V3(ProfessionalMEMStool)whichisthemotherboard compatible with all ST MEMS adapter boards. It is also possible to run UNICO offline (without the motherboard) forgeneratingconfigurations ofadvanced features likethe Machine LearningCore, Finite StateMachine, and pedometer.

The platform allows quick and easy setup of the sensors, as well as the complete configuration of all the registers and advanced features(such as the Machine Learning Core, Finite State Machine, pedometer, etc.) embedded in the digital output devices. The softwarevisualizes the output of the sensors in both graphical and numeric format, and allows the user to save or generally manage data comingfrom the device.

Examplesoftoolswhichsupporttheadvancedfeaturesare thefollowing:FIFOtoolthatallowstheuser tobufferdatawithahighlevel of flexibility and burst the significant data out when needed; Finite State Machine tool that allows the user to configure the statemachines, test their functionality and validate the program; Machine Learning Core tool that allows the user to configure a machinelearning core starting from the management of data patterns and labeling to setting and generating the configuration file to run thealgorithm; FFT tool that allows visualizing the Fast Fourier Transform of the output data; Pedometer tool that allows the user toconfigureand test the pedometer embedded in the device including an offline post-processing analysis;

## 3.4[**Weka**](https://www.cs.waikato.ac.nz/ml/index.html)

**Weka** [[8]](#_References)containsacollectionofvisualizationtoolsandalgorithmsfor[dataanalysis](https://en.wikipedia.org/wiki/Data_analysis)and[predictivemodeling](https://en.wikipedia.org/wiki/Predictive_modeling),togetherwithgraphicaluserinterfacesfor easy access tothese functions.[[](https://en.wikipedia.org/wiki/Weka_(machine_learning)#cite_note-%3A0-1)1]. Advantages of Wekainclude:

- Portability,sinceitisfullyimplementedinthe[Javaprogramminglanguage](https://en.wikipedia.org/wiki/Java_programming_language)andthusrunsonalmostanymoderncomputingplatform.
- A comprehensive collection of data preprocessing and modeling techniques.
- Ease of use due to its graphical user interfaces.

Wekasupportsseveralstandard[datamining](https://en.wikipedia.org/wiki/Data_mining)tasks,morespecifically,datapreprocessing,[clustering](https://en.wikipedia.org/wiki/Data_clustering),[classification](https://en.wikipedia.org/wiki/Statistical_classification),[regression](https://en.wikipedia.org/wiki/Regression_analysis),[visualization](https://en.wikipedia.org/wiki/Data_visualization),and[feature selection](https://en.wikipedia.org/wiki/Feature_selection).

Input to Weka is expected to be formatted according the Attribute-Relational File Format and with the filename bearing the **.arff** extension.AllofWeka'stechniquesarepredicatedontheassumptionthatthedataisavailableasoneflatfileorrelation,whereeachdatapoint is described by a fixed number of attributes (normally, numeric or nominal attributes, but some other attribute types are alsosupported). Weka provides access to [SQ](https://en.wikipedia.org/wiki/SQL)L [databases](https://en.wikipedia.org/wiki/Database)using [Java Database Connectivity](https://en.wikipedia.org/wiki/Java_Database_Connectivity) and can process the result returned by a databasequery. Weka provides access to [deep learning](https://en.wikipedia.org/wiki/Deep_learning) with [Deeplearning4j](https://en.wikipedia.org/wiki/Deeplearning4j).[[](https://en.wikipedia.org/wiki/Weka_(machine_learning)#cite_note-4)4] It is not capable of multi-relational data mining, but there isseparate software for converting a collection of linked database tables into a single table that is suitable for processing using Weka.[[](https://en.wikipedia.org/wiki/Weka_(machine_learning)#cite_note-5)5]Anotherimportantarea thatis currentlynotcovered bythe algorithmsincluded intheWeka distributionis sequencemodeling.

# 4 **Implementation**

## 4.1 **STM32CubeIDE**

![](RackMultipart20230414-1-bpee3o_html_7362cb28ff621b41.jpg)First,the program needs to be flashed on the board. Thisneeds to be donewith **STM32CubeIDE**.

## 4.2 **Unicleo-GUI**

After the program was flashed to the board, the sensors are initialized and ready to be used.

This is the interface of **Unicleo**. If the board is connected via USB (ST Link) then the Serial Port

![](RackMultipart20230414-1-bpee3o_html_a235300dc49e4db8.jpg)should be automatically selected,mineforexampleisCOM5. **S** _ **elect Connect** _.

The sensors list will pop up and show all available sensors. Choose the **LSM6DSOX (DIL24)**.

**Note**** that** there is also another sensor named LSM6DSO, that is the exact same sensor, but on the extender IKS01A3 and does not contain the Machine Learning Core.

![](RackMultipart20230414-1-bpee3o_html_acf24bb9fcb75aad.jpg)

After **Apply** has been selected, the following window will pop up. It shows the sensors of the IKS01A3 and the MKI197V1 along with their locations on the board **.**

![](RackMultipart20230414-1-bpee3o_html_5ee512d5fd4a3ae0.jpg)On the left the option for the visualization of the available sensors can be seen.

If _ **MLC** _ is chosen from the left, the user will be greeted by the following window. On the first block, named _ **Sensor Configuration** _, a custom _ **.ucf** _ dataset can be loaded. Bellow that are some _ **Example Algorithms** _, that, if chosen, they will be loaded and cording to the motion of the sensor, a different value will be shown on the _ **MLC Source Registers** _ bellow.

![](RackMultipart20230414-1-bpee3o_html_4fd838f6e01908d.png)The values will be displayed on the blocks on the right of _ **MLC0\_SRC** _.

## 4.3 **Machine Learning Core Example**

For example, here are the specifics of the _**Activity Recognition (Wrist)**_ algorithm.

According to the documentation of the algorithm:

- **1 = Stationary/Other**
- **4**** = ****Walking/Fast Walking**

- **8 = Jogging/Running**

![](RackMultipart20230414-1-bpee3o_html_d12a3dadae254d9a.jpg)

Before analgorithm can be selected, _ **Start** _ must be selected onthemainwindow, to activate the sensor.After that, the sensor will begin sending feedback.

![](RackMultipart20230414-1-bpee3o_html_1340bc3d15f01a05.jpg)

Now, instead of using one of the example algorithms, a custom _ **.ucf** _ file will be created and used to show the configurations that need to be set. To do that, data must be logged into a dataset based on each action that one wants to add to the Decision Tree. The log will be created in Unicleo (in _ **.txt/ .csv** _ format) and then _ **Unico** _ will be used to create the _ **.ucf** _ file. Then the _ **.ucf** _ file will be loaded to _ **Weka** _ to create _ **Decision Tree** _ and after that it will be loaded back to _ **Unico** _.

_In order to log the data, the_ _ **Datalog** _ _option needs to be chosen on the left of the main window. Next choose the sensors that you want to log into a file from both_ _ **Data** _ _and_ _ **Datalog period source** _ _ad set a file for said activity._

![](RackMultipart20230414-1-bpee3o_html_390dd78f05d2982d.jpg)_Here, motions will be monitored for approximately 1 minute and saved in a dataset file_ _ **karate.csv** __._

The same steps will be followed for the dataset _ **boxing.csv** _ _ **.** _

![](RackMultipart20230414-1-bpee3o_html_cfb2f707a53db337.jpg)

## 4.4 **Unico-GUI**

Now_ **Unico** _needstobeusedinordertocreateafilethatcanbereadby_ **Weka** _.

SinceUnico cannot be used withthe shield _ **IKS01A3** _, it willhave to be used inoffline mode. That means thatit will not connect with the board, but the datasets will be able to be loaded in order to extract an _ **.arff** _ file.

![](RackMultipart20230414-1-bpee3o_html_41c07e993177a272.jpg)Selectinthe **iNemo**** Inertial ****Modules** the_ **STEVAL-MKI197V1** __**(LSM6DSOX)**_sensoranddeselectthe _ **Communication with**__ **the** __**motherboard** _option.Thenclickon_ **Select**__ **Device** _.

![](RackMultipart20230414-1-bpee3o_html_d5e43e8f5351379b.png)After the main window shows up, select _ **MLC** _ on the left and the _ **Machine Learning Core** _ [[9]](#_References) window will open.

Now each one of the datasets will be loaded and set the _ **Class Label** _ as _ **boxing** _ for the **Boxing** Dataset and **Karate** for the Karate Dataset.

![](RackMultipart20230414-1-bpee3o_html_9d6edf35e652e64d.png)

![](RackMultipart20230414-1-bpee3o_html_a8342d45b358853f.png)Then, the _ **Configuration** _ Tab must be selected. Here are the options that were given to the _ **Decision Tree.** _

The chosen options were all the _ **Signed** _ options _ **ACC\_X, ACC\_Y, ACC\_Z, GY\_X, GY\_Z** _for the _ **Mean** __,_ _ **Variance**__ ,_ _ **Energy** _ _and_ _ **Peak** __**to**__ **Peak** _features.

![](RackMultipart20230414-1-bpee3o_html_ccbf8d63f5ee2d41.png)

![](RackMultipart20230414-1-bpee3o_html_d74758057bfb9b97.png)

Save the file as **.arff** and choose the desired output values of the **Decision Tree**.

- **1** was set for **Karate** and **2** for **Boxing**.

![](RackMultipart20230414-1-bpee3o_html_1f7abf63ca235975.png)

## 4.5 **Weka**

![](RackMultipart20230414-1-bpee3o_html_46cf97b5f440dcbd.png)Open _ **Weka** __ **Explorer** _ and load the **sports.arff** file that was created above:

Select the _ **Classify Tab** _ and then _ **Choose** _ on the _ **Classifier** __ **Block** _ and select the _ **J48** _ in the tree section. The _ **Cross-Validation Folds** _ option was set on 10, as default, since it gives an approximate 98% result.

![](RackMultipart20230414-1-bpee3o_html_a27c7934742ad8e0.png) ![](RackMultipart20230414-1-bpee3o_html_9dad19c6c832500e.png)

![](RackMultipart20230414-1-bpee3o_html_d7de328c6c5f3f61.jpg)The _ **Decision Tree** _ has been generated, but in order to load it into _ **Unico** _ to create the _ **.ucf** _ file, the selected text, as seen in the image below (_ **The tree itself** _), needs to be copied and pasted into a _ **.txt** _ file.

![](RackMultipart20230414-1-bpee3o_html_2bbe01988000b5d1.png)

Andtoseethe_ **Decision** __ **Tree** _,right clickonthe _ **Result List** _and select _ **Visualize Tree** _.

![](RackMultipart20230414-1-bpee3o_html_ac95af09cb85817f.png)

![](RackMultipart20230414-1-bpee3o_html_2e65155471e368a7.png)Now the **.txt** file can be loaded into the _ **Unico** _ window that was left intact a while ago.

And save the file as _ **sports.ucf** _.

![](RackMultipart20230414-1-bpee3o_html_e1ee0a4d7a0ddcdb.png)

![](RackMultipart20230414-1-bpee3o_html_77b254ff3789329c.png)Now the **sports.ucf** file can be loaded into **Unicleo** in the **MLC** (Machine Learning Core) option.

Here the changes can be seen as the value changes to **0x01** in _ **MLC0\_SRC** _ when the board mimics the action for **Karate**.

![](RackMultipart20230414-1-bpee3o_html_ac5bbb5a653be640.png)

And here _ **MLC0\_SRC** _ changes to _ **2** _ when the board mimics the action for **Boxing**.

![](RackMultipart20230414-1-bpee3o_html_14b4f590096f31d.png)

## 4.6 **Video Demonstration**

[Here is a video demonstration of the](https://www.youtube.com/watch?v=m6ylfVGBezo)_ **LSM6DSOX** _ sensor changing value from _1_ to _2_ when it recognizes the different actions.

[**Machine Learning Core LSM6DSOX Demonstration**  **||**  **ISCA Lab**](https://www.youtube.com/watch?v=m6ylfVGBezo)[[10]](#_References)

[![](RackMultipart20230414-1-bpee3o_html_c4fa1f36a793ba36.jpg)](https://www.youtube.com/watch?v=m6ylfVGBezo)

# 5 **Falling Detection Algorithm**

This example is made for the use-case of detecting the Fall of the device from **Low** or **High altitude**. It demonstrates the change on the registers of the Machine Learning Core of Unicleo when the board is free-falling from both a low and high altitude.

In order to create the datasets for both falls, a scenario was created for logging the Data for each case.

## 5.1 **Creation of Falling Detection Datasets**

**Low and High Altitude Fall**

For both the **Low** and **High** altitude fall, 2 datasets were created with the board mimicking each fall, in a safe environment, from a height of approximately 90 centimeters for the **Low** fall and approximately 2.5 meters for the **High** fall.

For this scenario, the mechanism that was created consisted of a small rubber rope attached to a thin rope, used to avoid damage from the force of the velocity, whereas the rope was used to simulate the fall by hand.

LogsweretakenfromeachscenarioandthenloadedintothesameDecisionTreeinordertohaveasingleconfigurationfordetectingbothFalls at the same time.

Inthevideoshownbelow,thechangesoftheregistersmaynotbeexactattimes(changingfromLowtoHighwhenfalling)and that is because of the bounce of the board due to the rubber that was used to simulate a safe Fall.

Since the algorithm detects the data mostly from the accelerometer, when falling it may detect a greater value, but when the rubberbounces back it simulates the velocity of a Low altitude fall ,and so, it detects it as such. In a real-world scenario, where the board is not in a safe environment, the fall would be one-way and the board wouldn't bounce, since it would most likely not be attached to anything,except maybe a cable.

In that scenario, the algorithm would detect only one Fall, and the prediction would be more precise.

Although, as most uses of a Machine Learning algorithm, many datasets need to be logged and merged for either the **IDLE** state of the board or the **Falling Detection**. This will lead to a more precise algorithm with a variety of predictions and broader spectrum of detections for different scenarios, angles and falls.

## 5.2 **Project Equipment**

![](RackMultipart20230414-1-bpee3o_html_31fe78565dcf9416.png)

## 5.3 **Video Demonstration**

This video demonstrates the different fall detections that the Machine Learning Core predicts from the output of the **Accelerometer** and **Gyroscope** of the **LSM6DSOX** Sensor.

MLCSourceRegister_ **0x70 MLC\_SRC** _ values in Unicleo:

- **0x01 -\>** Value for the board being **IDLE**
- **0x04 -\>** Value for the board falling from a **Low Altitude**
- **0x08 -\>** Value for the board falling from a **High Altitude**

[**Falling Detection with Machine Learning Core STM MEMS Sensor LSM6DSOX || ISCA Lab** [11]](#_References)

[![](RackMultipart20230414-1-bpee3o_html_5212357644af4464.jpg)](https://youtu.be/tSlJXf_sjQc)

## 5.4 **Possible Real-World Use Scenarios**

A possible scenario of such a use case, such as the one shown above, would be the use of a device with capabilities of **Bluetooth** connectivity.

Such a scenario would prove to be a more efficient solution to provide wireless data of a device in a monitoring database or simple real-time feedback to a computer.

The device that was available for this demo did not have **Bluetooth** connectivity and so such a scenario was not recorded, but would bare the same results.

For example, both **Unico** and **Unicleo** have an option to enable **Bluetooth** via their GUI interface. After that, the scenario of logging the data in a remote computer/database could be explored, in the future, for providing information of devices in case of their required position is compromised.

# 6 **Falling Detection Algorithm With NUCLEO-WL55JCx and SHUBv3**

This section covers the demonstration of a real use-case scenario for the Falling Detection Algorithm that was described previously. The microcontroller that was used is the **NUCLEO-WL55JCx** , in addition with the **Sensor Hub v3** ( **SHUBv3** ) and on top of that is connected the **MKI197v1** adapter board that uses the **LSM6DSOX** sensor to detect any movement of the board and provide output based on the dataset that was given to it.

In contrary with the previous demonstration, this board is not in the supported devices of the **X-NUCLEO-MEMS1** package, and thus a custom firmware needed to be developed, to enable communication with the **SHUBv3** and the **MKI197v1** adapter board.

To enable the **Machine Learning Core** in this project, the LSM6DSOX had to be included in the project manually, along with the **MLC** option, provided by ST in their [GitHub](https://github.com/STMicroelectronics/STMems_Standard_C_drivers/blob/master/lsm6dsox_STdC/examples/lsm6dsox_mlc.c) repository.

In order to be able to monitor and log the data for the decision tree, a different firmware was developed, that enabled the **Unicleo-GUI** application and allowed the DataLog of the simulation data.

The steps that were followed were exactly the same as shown in the sections 4.1 - 4.5, with the exception that the .ucf file was converted into a header (.h) file and was used in the firmware. That way the output of the **MLC** option was based registers that were given when the Decision Tree was created.

## 6.1 **Equipment**

- WL55JCx (x = 1 or 2)

![](RackMultipart20230414-1-bpee3o_html_2a41f437adc2f8f9.jpg)

- SHUBv3

![](RackMultipart20230414-1-bpee3o_html_5645535016ffba3d.png)

- WL55JCx + SensorHubV3 + MKI197v1

![](RackMultipart20230414-1-bpee3o_html_ab89092797f15b26.png)

## 6.2 **Decision Tree Generation**

The decision tree was generated based on the datasets that were logged for this project, which include:

1. **Idle Position**
2. **Low Fall Detection**
3. **High Fall Detection**

The tree was generated with WEKA, using **10-Fold Cross-Validation** and the **J48** algorithm.

### 6.2.1 **K-fold Cross-Validation**

Cross validation is one of the techniques used to test the effectiveness of machine learning models and avoid overfitting. It is a re-sampling procedure used to evaluate a model, and with the option of 5 or 10 folds (e.g., 10-fold Cross-Validation), the procedure can be executed without using excessive computation sources in order to train the model.

With K-fold Cross-Validation we divide the data set into k-subsets and the procedure is repeated k-times. A training set is created, which consists of k-1 subsets and a testing set consisting of the remaining subsets.

For example, in 10-fold Cross-Validation, the procedure would perform a total number of ten times, reserving 9/10 parts for training and the remaining 1/10th for testing, each time reserving a different tenth for testing.

### 6.2.2**J48 Algorithm (also known as C4.5)**

The C4.5 algorithm for building decision trees is implemented in Weka as a classifier
called J48. The decision trees from this algorithm are generated using the concept of **information entropy** and can be used for **classification**.

- **Information entropy** is a measure of how much information there is in some specific data. It isn't the length of the data, but the actual amount of information it contains.

For example, **one text file could contain "Apples are red." and another text file could contain "Apples are red.**** Apples are red.**

- **Classification** refers to a supervised learning concept which basically categorizes a set of data into classes.

Classifiers, like filters, are organized in a hierarchy: J48 has the full name
weka.classifiers.trees.J48. The classifier is shown in the text box next to the Choose
button: It reads J48 –C 0.25 –M 2. This text gives the default parameter settings for this
classifier.
C4.5 has several parameters, by the default visualization (when you invoke the
classifier) only shows –c ie. Confidence value (default 25%): lower values incur heavier
pruning and -­‐M ie. Minimum number of instances in the two most popular branches
(default 2). The full set of J48 parameter settings are explained here:
[http://weka.sourceforge.net/doc.dev/weka/classifiers/trees/J48.html](http://weka.sourceforge.net/doc.dev/weka/classifiers/trees/J48.html%20) .

_On the left is the extended Decision Tree created by WEKA with the J48 algorithm, and on the right is the Classifier Visualization._

![](RackMultipart20230414-1-bpee3o_html_154eb4e2fdba322c.png)

_Below is the Decision Tree visualization offered by WEKA._

![](RackMultipart20230414-1-bpee3o_html_270c4a1e06298e1.png)

## 6.3 **Performance**

For the configuration that was given for this decision tree, the following test summary was given by the WEKA toolchain:

| **Correctly Classified Instances** | 51 | 80.9524 % |
| --- | --- | --- |
| **Incorrectly Classified Instances** | 12 | 19.0476 % |
| **Kappa statistic** | 0.7056 |
 |
| **Mean absolute error** | 0.1263 |
 |
| **Root mean squared error** | 0.3398 |
 |
| **Relative absolute error** | 29.1923 % |
 |
| **Root relative squared error** | 73.0292 % |
 |
| **Total Number of Instances** | 63 |
 |

## 6.4 **Video Demonstration**

The video demonstrates the values of the registers that are generated by the Machine Learning Core based on the header file that was included in the firmware.

[**Falling Detection with Machine Learning Core Sensor LSM6DSOX - WL55JCx + SensorHub || ISCA Lab**](https://www.youtube.com/watch?v=GsexEODn7TA)[**[12]**](#_References)

![Picture 16](RackMultipart20230414-1-bpee3o_html_8ef2d51c3eeb4d3e.gif)

# 7 **References**

1. [**B-L072Z-LRWAN1**](https://teicrete-my.sharepoint.com/personal/tp4591_edu_hmu_gr/Documents/B-L072Z-LRWAN1), [https://www.st.com/en/evaluation-tools/b-l072z-lrwan1.html](https://www.st.com/en/evaluation-tools/b-l072z-lrwan1.html)

2. [**X-NUCLEO-IKS01A3**](#_X-NUCLEO-IKS01A3), [https://www.st.com/en/ecosystems/x-nucleo-iks01a3.html](https://www.st.com/en/ecosystems/x-nucleo-iks01a3.html)

1. [**STEVAL-MKI197V1**](#_STEVAL-MKI197V1), [https://www.st.com/en/evaluation-tools/steval-mki197v1.html](https://www.st.com/en/evaluation-tools/steval-mki197v1.html)

1. [**I3C**](#_LSM6DSOX_Sensor_Initialization), [https://en.wikipedia.org/wiki/I3C\_(bus)](https://en.wikipedia.org/wiki/I3C_(bus))

1. [**X-CUBE-MEMS1 Firmware Package**](#_Firmware),

[https://github.com/STMicroelectronics/X-CUBE-MEMS1](https://github.com/STMicroelectronics/X-CUBE-MEMS1)

1. [**Unicleo-GUI**](#_Unicleo-GUI), [https://www.st.com/en/development-tools/unicleo-gui.html](https://www.st.com/en/development-tools/unicleo-gui.html)

1. [**Unico-GUI**](#_Unico-GUI_1), [https://www.st.com/en/development-tools/unico-gui.html](https://www.st.com/en/development-tools/unico-gui.html)

1. [**Weka**](#_Weka), [https://www.cs.waikato.ac.nz/ml/index.html](https://www.cs.waikato.ac.nz/ml/index.html)

1. [**Unico's Machine Learning Core User Guide**](#_Unico-GUI),

[https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiD8Jjfh5T6AhV-hf0HHevHC7AQFnoECB8QAQ&url=https%3A%2F%2Fwww.st.com%2Fresource%2Fen%2Fuser\_manual%2Fcd00297387-unico-gui-stmicroelectronics.pdf&usg=AOvVaw3QMf286kZJigIy6RPmtIM2](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiD8Jjfh5T6AhV-hf0HHevHC7AQFnoECB8QAQ&url=https%3A%2F%2Fwww.st.com%2Fresource%2Fen%2Fuser_manual%2Fcd00297387-unico-gui-stmicroelectronics.pdf&usg=AOvVaw3QMf286kZJigIy6RPmtIM2)

1. [**Machine Learning Core LSM6DSOX Demonstration || ISCA Lab**](#_Video_Demonstration),[https://www.youtube.com/watch?v=m6ylfVGBezo](https://www.youtube.com/watch?v=m6ylfVGBezo)

1. [**Falling Detection with Machine Learning Core STM MEMS Sensor LSM6DSOX || ISCA Lab**](#_Video_Demonstration_1), [https://www.youtube.com/watch?v=tSlJXf\_sjQc&feature=youtu.be](https://www.youtube.com/watch?v=tSlJXf_sjQc&feature=youtu.be)

1. **Falling Detection with Machine Learning Core Sensor LSM6DSOX - WL55JCx + SensorHub || ISCA Lab,** [https://www.youtube.com/watch?v=GsexEODn7TA](https://www.youtube.com/watch?v=GsexEODn7TA)

6
