# Chromatic3D
This Klipper full-color printing module (Chromatic3D)achieves high-precision color printing on ordinary FDM 3D printers by integrating hardware control and software collaboration. Here is a detailed analysis of its core working principles:

### I. Hardware Architecture and Workflow
#### 1. Core Components of the Inkjet System
- **Nozzle Design**: Micro-piezoelectric nozzles modified from Epson-compatible ink cartridges are used. Each nozzle contains hundreds of nozzles with a diameter of approximately 20μm, and the ejection of ink droplets is controlled by the vibration of piezoelectric ceramic sheets.
- **Color System**: CMYK (Cyan, Magenta, Yellow, Black) four-color inks are employed, and mixing them in different proportions can achieve 16.7 million colors.
- **Drive Mechanism**: The ESP32 main control board controls the nozzle driver chip through PWM signals, precisely adjusting the size of ink droplets (2 - 10 nanoliters) and the ejection frequency (up to 50kHz).

#### 2. Integration with the Printer
- **Mechanical Structure**: The nozzle assembly is installed in front of the extruder head via a 3D-printed bracket, maintaining a fixed spacing (usually 5 - 10mm) and moving synchronously with the print head.
- **Circuit Connection**: The ESP32 communicates with the Klipper main control unit via USB or serial port, receiving inkjet instructions during the printing process.

### II. Software Collaboration Mechanism
#### 1. Extension of Klipper Firmware
- **G-code Interception**: By modifying the `gcode_macro` module of Klipper, specific G-codes (such as `M106`) are intercepted and forwarded to the inkjet controller.
- **Position Synchronization**: Real-time coordinates of the extruder head are obtained, and the position that the nozzle should reach is calculated in combination with the preset offset.

#### 2. Color Processing Flow
1. **Model Preprocessing**: Slicing software (such as Cura/PrusaSlicer) divides the 3D model into layers and assigns colors to the surface of each layer.
2. **Color Mapping**: A custom plugin converts RGB colors into CMYK ink ratios and generates G-code containing inkjet instructions.
3. **Instruction Integration**: After each layer is printed, the command `M106 S[intensity] R[red] G[green] B[blue]` is inserted to trigger inkjetting.

### III. Principles of High-Precision Implementation
#### 1. Ink Drop Positioning Technology
- **Biaxial Compensation**: The position of the nozzle is fine-tuned in the X/Y axis directions to compensate for the physical offset between the extruder head and the nozzle.
- **Interlayer Alignment**: Klipper precisely controls the Z-axis height to ensure that the inkjet layer fits perfectly with the printed layer, with an error of less than 0.05mm.

#### 2. Resolution Optimization
- **Dynamic Ink Drop Control**: The density of ink droplets is automatically adjusted according to the size of the printing area. Smaller ink droplets (2 - 5nl) are used in fine areas to improve detail performance.
- **Overscan Technology**: Multiple inkjet operations are performed in key areas, and color saturation and uniformity are enhanced through offset superposition.

### IV. Work Timing Control
1. **Extrusion Stage**: The printer extrudes plastic filaments and forms model layers according to the conventional process.
2. **Lifting Stage**: The Z-axis rises to a safe height (to avoid scraping), and the nozzle moves to the starting position.
3. **Inkjet Stage**: Colorful inks are sprayed along the preset path, covering the cured plastic layer.
4. **Curing Stage**: Optionally, a UV lamp can be configured to quickly cure the ink, preventing smudging during the printing of subsequent layers.

### V. Key Technical Challenges and Solutions
| **Challenges** | **Solutions** |
|----------------|-----------------------------------------------------------------------------|
| Ink Drying Time | Use quick-drying dye-based inks and heat the print head (50 - 60°C) to accelerate water evaporation. |
| Nozzle Clogging | Design an automatic cleaning program, regularly flush the nozzles with cleaning solution, and keep them sealed when idle. |
| Color Calibration | After printing test patterns, analyze color deviations through computer vision and automatically adjust the ink ratio. |
| Material Compatibility | Pretreat the printing surface (such as spraying primer) to enhance ink adhesion and avoid color fading. |

### VI. Performance Indicators
- **Color Accuracy**: Supports 24-bit true color (16.7 million colors), with a color deviation ΔE < 3 (professional display level).
- **Resolution**: The horizontal resolution can reach 360dpi (ink droplet spacing of 0.07mm), and the vertical resolution is determined by the layer thickness (usually 0.1 - 0.2mm).
- **Printing Speed**: The printing speed of single-color layers is not affected, while the printing time of color layers increases by approximately 20 - 30% due to the inkjet process.

### VII. Application Scenarios
- **Product Prototyping**: Directly print full-color appearance models without the need for post-coloring.
- **Educational Field**: Create vivid teaching models (such as anatomical models, geographical landscapes).
- **Artistic Creation**: Achieve integrated printing of complex color gradients and patterns.

Through this combination of software and hardware, the Klipper full-color printing module enables ordinary 3D printers to obtain professional-level color printing capabilities without significantly increasing costs. 
