# -Physical-Verification-using-SKY130
## Day 3 Design Rule Checking 

### Fundamentals of design rules

Physical verification of masks generated from layout data vs foundry process rules specific for each foundry and process node.  

The masks are used for defining the components (transistors , resistor, caps) build during fabrication steps which are deposited/exposed to light/etched on a substrate - silicon wafer. 

The precision of the equipment, materials used, timings, cleanliness in the fabs will generate some constrains for the geometries used for masks. The defects (spot defects)  will not be eliminated completely but they will have a normal distribution and will generate a process yield. 

The DRC scope is to maintain the same failure rate (ppm-parts per million) on all wafers so we can rely on the statistics given by the fab. DRC will keep the manufacturability of the wafer , if not fulfilled most probably the fab will refuse to produce the component.

Skywater130 DRC: https://antmicro-skywater-pdk-docs.readthedocs.io/en/latest/rules.html

### Back-end Metal layer
- Minimum width: for metal, for implants, for features size (polysilicon layer) that defines the transistor gate/ length (ex: sky130 poly width >150nm)
- Spacing which usually generates short circuits (ex: if wires go parallel for a longer distance need more spacing) , wide-spacing rule (if a metal area is bigger that addition spacing is required for the neighboring wires) 
- Notch rules are usually spacing rules and they are merged together  
- Area rules (min, max) : for metals usually eliminates the possibility of delamination of metal layers and influences the vias parameters , for implants/diffusion to have enough material exposed .  
- Minimum hole area for metals so the metal will not fill the need holes
- Contact cut for vias need minimum hole to aligning the tot/ bottom metals with the via hole in the oxide layer , minimum surrounding area on the metal layers. 
Magic draw vias as single layers that comprise top+bottom poly layers, metal contact cut and local interconnect 

### Interconnect rules
It is specific to skywater 130 process , many other processes go from polysilicon layer directly to the first aluminum layer. Polysilicon has high resistance , sometimes is coated with some special high resistive material but still not good for routing.
Because the resistivity of the interconnect layer (Ti) is still higher some more like design guideline must be fulfilled : aspect ration of uncontacted local interconnect >1:10 so no long wires.
All terminals should be contacted to metal 1.

### Front-end
Most of designers will use standard cells and PDK will take care automatically . 
Parameters of transistor :channel width , polysilicon with , endcap length , source/drain length., gate poly to gate spacing
Poly to diffusion spacing , not to form a transistor by mistake. 
Rules are defined also for implant areas for wells and taps, wells connected to different voltage nets,  deep n-well.
High voltage rules for transistors that have high gate oxide and special well implants will increase almost all geometrical sizes of a transistor .
Reistors, that can be created from polysilicon material or p-diffusion materiaal, have also specifc rules .
Cpacitor are :
- Varactors : that have a transitor structure -capacitance between gate and well - so the have similar rules
- MOScap: capacitors formed by gate of FET with source and drain tied toghether , drc rules of MOSFET
- Vertical parallel caps, Metal Oxide Metal, figer s of metal layers follow rules of metal layers
- Metal insulator Metal (MiM) caps on metal layers with thin oxid between them , specific rules . Sky130 specifies dual layer caps
Diodes - usually is a parasitic component , formed in magic with ID layers
Fixed layout devices - usually has fixed design rules from the vendor in the standard cel. Bipolr Tranz, photodiodes , sram cells, etc.

### Miscelaneos rules:
- Off-grid - not very comon 
- Angle limitation 
- Seal ring : outer perimeter of a chip design - is treated like a fixed layout device but will vary when design will be resized 
- Latchup rules :due to parasitic bipolar transistor formed betwen taps, wells and substrate - the rules will generate minum disctance bewteen tap connection and any diffusion zone. 
- Antenna rules: long wires can capture charges and can generate high voltages if they are not connected to a return path or load. The high voltage will penetrate components with a certain brakedown voltage , usualy the transitor gate. Usually not missed by the designers this can apair during manufacturing .Thsi can be mitigated also with placement of diodes. 
- Stres rules - related to material delamination by mechanical processes . Critical points are corners, edges usaaly containing the pad area.
- Desnity rules: is necesary to mantain a flat surface on the layers and with higher desnity of metal conections this is achieved easier.
Fill patterns are used to mitigate this . For analog design the patterns can influence the circuit parasitics. 

### Reomanded , manufacturing and ERC rules
- Recomened rules - will not reject the layout by foundry 
- Manufacturing rules - causes rejection of layout by foundry 
- Electrical rule checks - cober lectrical fail mecanisms like lectromigration , overvoltage conditions 

### Day3 Lab
#### Width and spacing rules 
Clone the exercise folder from ```git clone https://github.com/RTimothyEdwards/vsd_drc_lab.git```
![](Day3/3-0.png)
Selecting menu DRC->DRC Report(?) erros can be seen in the comand window, erros will be repoted just for selection .
Use 'b' key to see dimension:
- microns: the size of the selected elemen (error indicated metal width 0x14 > 0.06 curent width)
- lambda :
- internal: minimum manufaturing grid
Grid features can be selected from menu Window. Set grid x.xx , Grid on, Snap-to-grid etc
Exercise 1a- line width: we can use filling the area with the mouse or in comand line ('''box width 0.14um / paint m2''')
![](Day2/3-1.png)
Exercise 1b - space width: solved by moving the area away with keypad arrows (4<-,6->,8 up, 2 down)
Exercise 1c - 2x metal spacing: rsolved similar with 1b moving the small component
Exercise 1d - notch error : usgae of menu Cell->Strech up action is used to widthen the notch or Shift +keypad arraows 
Strech of element can be don selecting a portion of a plane , pressing a and using comand ```stret e 1.1.6um``` (e - est).
![](Day2/3-2.png)
Exercise 2a via size - solved with strech 
Exercise 2b multiple via - used in designs with multiple via contacts . ```feedback why``` comand can show the possible via conection . ```feeback clear``` will reset the view.
If the area is to small to feet a contact we will get a feedback error.
```cif see``` will show the layers used for via contact.
![](Day2/3-3.png)
Exercise 2c - via overlap. metal layer must be generated aroudn the via hole and dimension acording to the rules .
Exercise 2d - autogenerate via: Using wiring tool hitting the space bar.  Wiring can be generated and presing Shit+ left mouse click and Shift +right mouse click we can jump to high/lower layers. 
When going to upper layers and the width rule incrieses magis is generating wider traces. With ```cif see comand```  
![](Day2/3-4.png)

# Acknowledgements
- [R. Timothy Edwards](https://github.com/RTimothyEdwards)
- [Kunal Gosh](https://github.com/kunalg123)
- [Kunal Ghosh](https://github.com/kunalg123)
- [VSD-IAT](https://vsdiat.com/)
