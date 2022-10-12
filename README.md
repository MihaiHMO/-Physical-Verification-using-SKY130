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


# Acknowledgements
- [R. Timothy Edwards](https://github.com/RTimothyEdwards)
- [Kunal Gosh](https://github.com/kunalg123)
- [Kunal Ghosh](https://github.com/kunalg123)
- [VSD-IAT](https://vsdiat.com/)
