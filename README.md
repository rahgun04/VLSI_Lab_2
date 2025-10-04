##### Imperial College London, Department of Electrical & Electronic Engineering


#### ELEC70142 Digital VLSI Design

### Lab 2 - Understanding Layout

##### *Peter Cheung, v1.0 - 9 October 2025*

---
### Objectives
---
By the end of this laboratory session, you should be able to do the following.
* Understand the different mask layers that make up the layout of an inverter.
* Manually extract the circuit schematic of a 12-transistor logic gate from its layout.
* Use Cadence's Virtuoso tool package to connect up a 4-bit LFSR circuit from Lab 1.
* Use Cadence's Calibre tool package to verify that your layout passes DRC and LVS checks.
* Verify that your "hand-wired" circuit works properly through simulation.

---
### Task 1 - Deep Dive into Inverter Layour (15-20 minutes)
---
The purpose of this task is to get you to understand the different mask layers that make up a simple inverter from the layout.  This helps you to appreciate the fabrication process and the physical domain of VLSI design.

**_Step 1: Launch Virtuoso_**

Log onto the teaching server ee-mill1 or ee-mill2 (depending on your group number), and set up to use *_pdk tsmc65LP_* as before.

Launch Virtuoso layout editor in the background by typing:
```bash
cadence&
```
A Virtuoso message window will appear. You are now runnig the Cadence Virtuoso package in the background.

**_Step 2: Fetch the inverter standard cell from library_**

Go back to the Virtuoso message window and click **_Tools -> Library Manager ..._**.  
<p align="center"> <img src="diagrams/open_LM.jpg" width="1000" height="170"> </p><BR>

Select the TSMC's **_tcbn65lpbwp7t_9lm_** library.

>**TSMC process naming convention:** 
> * **_tc_** - technology characterisation
> * **_bn65_** - 65nm standard cell library
> * **_lp_** - low power option
> * **_bwp7t_** - bulk CMOS (bw), low threshold voltage (p) and 7-track height
> * **_9lm_** - 9 layers of metal

<p align="center"> <img src="diagrams/library_manager.jpg" width="800" height="550"> </p><BR>

**_Step 3: Examine mask layers for the inverter_**

From the list of standard cells in this technology library will appear. Select **_INVD0BWP7T_**.  This is one of the cells used in the LFSR4 circuit in Lab 1.

>**TSMC standard cell naming convention:** 

> * **_INV_** - inverter cell
> * **_D0_** - lowest output current capability (D6 is highest)
> * **_BWP7T_** - bulk CMOS (bw), low threshold voltage (p) and 7-track high cell library (7t)

Couble click the **_layout_** on right most column to view the layout of this inverter standard cell.  (See diagram.) You can also the short-cut **_"f"_** to fit the entire layout in the window.

You should now examine the different mask layers that are used to fabricate this inverter by:
1. The layout cellview window shows the inverter standard cell layout. On the left are ALL the mask layers associated with process. There are more than 50 masks required!
2. Click on **_Used_** tick box at the top to show ONLY the masks layers that are used for fabricating this standard cell.  (See diagram below.)
3. Click on **_V_** (red circle) to turn OFF all layers.
4. From top to bottom, turn on one layer at a time and observe how the inverter cell is "fabricated".
   
Discuss with your partner what you understand about how an inverter is fabricated.

<p align="center"> <img src="diagrams/layers.jpg" width="600" height="400"> </p><BR>

> **Name of the mask layers:**
> * NW - N-well
> * OD - oxide diffusion region, also called Active mask
> * PO - polysilicon gate
> * PP - p-type diffusion
> * NP - n-type diffusion
> * CO - contact
> * M1 - metal 1 
> * prBndry - boundary of the standard cell where it is joint to the next cell
> * text - text label for this cell (i.e. cell name)
> * M1 - cell input output pin locations on metal 1


### Task 2 - Extact Circuit from Layout (20-30 minutes)

The goal of this task is for you to learn how to intepret a layout and re-create the transistor schematic of a 12-transistor standard cell.

**_Step 1: Load the XOR gate standard cell_**

If the Library Manager window is not open, open it from the Virtuoso message window with: **_Tools -> Library Manager ..._**.

In the Library Manager window, select the XOR gate standard cell **_CKXOR2D0BWP7T_**.

> CKXORw - clock optimized 2-input XOR gate

Open the layout of this standard cell by double-clicking on **_layout_** view.  A layout window will popup and show the different mask layers for this cell. Use the **_f_** short-cut key to fit the layout within the window.

**_Step 2: Extract the circuit schematic from the layout_**

You and your lab partner are now required to extract from this gate layout all the transistors. Then connect them together to produce a transistor level schematic diagram.  You should label all transistors in the top row from left to right with **_odd_** designations (i.e. T1 to T11), and the bottom row with **_even_** designations (T2 to T12).  Put the schematic circuit diagram in your logbook.

**_Step 3: Sizing the transistors_**

* With the short-cut **_"k"_**, measure the size of all transistors and annotate them on your schematic.
* Check with another group near you whether you have the same circuit as they do.  
* Satisfy yourself that this cicuit fulfils the logic function of a 2-input XOR gate.

### Task 3 - Hand Place and Route the LFSR4 circuit (60-90 min)

The purpose of this task is for you to learn how to use Virtuoso for **layout editing**.  While you will not be designing layout of a gate from transistor up, you will still need to learn how to wire up synthesize, placed and routed modules and connect them to the pad ring.

The goal of this task is for you to take the Verilog netlist from Lab 1 and manually place them as a row of cells and connect the cells into the LFSR4 circuit.

**_Step 1: Set up the LAB_2 folder as a new library (i.e. project)_**

Before we start layout editing, we need to create a project (called a library in Cadence term) for this exercise. 

Create the project folder by:

*   Navigate to File -> New -> Library.
*   Name the library (say as LAB_2) and tick the "Attached to an existing technology library" radio button and click **Apply**.
*   In the pop-up window, select **_tsmcN65_** and click **Apply**.

<p align="center"> <img src="diagrams/lab2_1.jpg" width="1000" height="250"> </p><BR>

>What you have just done was to create a directory called **LAB_2** in your **_pwd_** (present working directory), initialize it for a Cadence layout design called **LAB_2**, and specify that the technology is **_tsmcN65_**.

If you use **_ls -l_** unix command to examine what has been created, you will see that LAB_2 folder with Cadence generated files ready to be receive a new layout in the TSMC 65nm process.

Copy from LAB_1/OUTPUTS folder, the synthesized, placed-and-routed netlist for LFSR4 to the LAB_2 folder:

```bash
cp ../LAB_1/OUTPUTS/lfsr4_soc.v .
```

**_Step 2: Import the synthesized LFSR4 circuit into Virtuoso_**

In the Virtuoso message window, use **_file -> import -> verilog_** command to open a Verilog import dialog window as below.

<p align="center"> <img src="diagrams/import.jpg" width="800" height="500"> </p><BR>

To successfully important the Verilog netlist from Lab 1, follow the steps below as show in the diagram above:
1. Use the browser button, select the netlist file *_lfsr4_soc.v_**.
2. Set LAB_2 (or whatever name you call this folder) as the target library (project) to be stored.
3. Add into the reference library list the technology file we are using. This contains information about the standard cells.
4. Specify that you can to important this Verilog netlist as both schematic circuit and to includes it functional specification.  This allows later for Layout vs Schematic verification.
5. Now go to "Global Net Options" menu at the top to change the window contents.
6. Remove the "!" characters in the power net names to "VDD" and "VSS".
7. Click OK to important the Verilog netlist and generate a schematic diagram.
8. Use the "f" short-cut to fit the entire schematic in the newly opened window.

You should see the schematic circuit for the PnRed LFSR4 module from Lab 1 as below.
   
<p align="center"> <img src="diagrams/schematic.jpg" width="1000" height="250"> </p><BR>

>You can zoom in and out of the schematic at the pointer location using scroll wheel of the mouse. 
>You can also pan across the schematic by click and hold the scroll button and drag the mouse.

Zoom into the bottom right component on the schematic as indicated above.  You will see the MSB D-FF of the LFSR *_sreg_reg[4]_*.  All the connections are automatically wired except for VDD and VSS. This is because the Verilog netlist does not include these signals.

<p align="center"> <img src="diagrams/zoomed_schematic.jpg" width="1000" height="250"> </p><BR>

**_Step 4: Add VDD and VSS connection to the schematic_**

We now need to tell the schematic about the VDD and VSS connections. 
* Use the "w" or wire shortcut, and "draw" two horizontal wires from VDD and VSS by right-click at start and end of wire. Use ESC to terminate each wire.
* Use the "l" or label shortcut, and enter VDD as label, and click on the wire to be labelled. So the same for VSS.
* Repeat this on all components.
  
>A quicker way to do this is to select the two wires and their labels, use "c" shortcut to cut the entire group and click at a new location to paste.  Try this yourself.
>If you make a mistake, you can use the **_"u"_** undo shortcut to undo your previous actions.

Once you have added the VDD and VSS wires to all components, click the **_check and save_** icon (third from the left with a green tick).  You will see one warning.  The warning is because the imported schematic created a cross connection at the output.  You can more the output cross wire down a bit to form a T-junction and not a cross junction. When you **_check and save_** again, the warning will disappear and the schematic is now correct nad ready to be used a reference for LVS verification later.

>We know the circuit represented by the schematic is a faithful representation of the intended circuit because it was generated from a simulated netlist from Lab 1.

**_Step 4: Preparation for manual layout of standard cells_**

The next step is to create a layout view of the LFSR4 circuit.  
1. In the main message window, create a new layout design with: **_File -> New -> Cellview_**.
2. In the "New File" window, specify details about this new layout file according to the diagram below.
3. In the new layout window, import the standard cells in the schematic with: **_Connectivy -> Generate -> All from Source ..._**. (See diagram below.)

<p align="center"> <img src="diagrams/generate_layout.jpg" width="1000" height="300"> </p><BR>

4. A "Generate Layout" dialogue box will pop up. Select the I/O pin section at the top of the window as shown in diagram below. This is to tell the layout which layer I/O pin of the standard cells are on.
5. Select M1 to say that all I/O pin are on metal 1 layer.
6. Click the "Create Label as" radio button and then click "Option".
7. Another window called "Set Pin Label" dialogue box will pop up.  Select both "Same As Pin" button.
8. Click the two "OK" buttons.

<p align="center"> <img src="diagrams/pin_section.jpg" width="600" height="400"> </p><BR>

After  these steps, you should see a new layout window pop up with a purple box at the top and all 7 standard cell instances inserted on the layout canvas as bounding rectangles.

**_Step 5: Floorplanning_**

The goal of this step is to arrange the cells in a rough floorplan as shown in the diagram below.  The rationale is that the inputs signals are on the left, the outputs are on the right. The cells are placed such that their output pins are close to the input pins to which they are connected.

<p align="center"> <img src="diagrams/floorplan.jpg" width="600" height="200"> </p><BR>

Before we can move the standard cells to match this floorplan, we need to do three preliminary steps:
1. **Arrange the schematic and layout windows** - resize and move the schematic and layout windows side by size. The layout window should be wider. If you now select a cell in one window, it will be highlighted on the other window.  In this way, you can identify which component is which.
2. **Change the PnR boundary** - Click on the purple bounding box (which will turn yellow) and use the "q" shortcut command to bring up the "Edit PR Boundary Property" dialogue box.  Change the bounding box coordinate to (0 0) (0 5) (50, 5) (50, 0).
3. **Set snap grid to 5nm** - Use Option -> Display to bring up the "Display Option" dialogue box and change the X and Y snap spacing to 0.005.

<p align="center"> <img src="diagrams/bb_snap.jpg" width="900" height="200"> </p><BR>

Select one cell at a time and drag and place it inside the PnR area (purple box) according to the floorplan diagram shown earlier. 

>Note that you can only move a component horizontally and then vertically in two separate steps, but not diagonally.

We next expose the actual layout of the cells with the **_"SHFIT-f"_** shortcut command.  
The result of this floorplanning step is shown in the diagram below.

<p align="center"> <img src="diagrams/floorplan_layout.jpg" width="1200" height="150"> </p><BR>

**_Step 6: Manual Placement_**

The next step is to place each cell at the final precise location. This action needs to be extrememly precise and require you to use the mouse to enlarge the layout so that all geometric feature is clearly seen.  

The most useful command here is **"a"**, the align shortcut command.  Let us now place the left-most cell (the delay cell) abutting the inverter cell:
1. Zoom into the top right corner of the left-most cell. 
2. Select the left-most cell (delay). The cell will be highlighted in a **WHITE** bounding box.
3. Press **a** command. The curson now has an alignment icon attached to it. You are now in alignment mode.
4. Click on the bottom edge of the metail 1 VDD wire (which will be highlighted).
5. Click on the bottom edge of the metail 1 VSS wire. This step will align precise the two cell horizontally.
6. Select the left cell again and press **a**.  Then click on the thinner purple verticle line of the left cell as the source location.  This verticle line marks the PnR boundry of the cell for abutment.
7. Click on the purple line on the inverter cell. This will move the right boundary of the delay element to the left boundary of the inverter. 

>Note that you can move cells closer together by selecting a cell and drag it to a new location. This allows you to see two cells in the same window for ease of alignment. 

You have now successfully placed two cells together.  Repeat this for all cells to form a perfectly aligned and placed line of standard cells as shown below.

<p align="center"> <img src="diagrams/placement.jpg" width="1200" height="150"> </p><BR>

Before we finish this placement step, we need to add to both ends of this layout a tap cell.  The tap cell connects the n-well (for p-type transistors) to the VDD power rail, and the substrate (for n-type transistors) to VSS (i.e. GND).  Such connections are NOT included in the Verilog netlist.

Use the **"i"** shortcut command to pick up an instance. This will pop up a **"Create Instance"** dialogue box. Fill this in as shown below:

<p align="center"> <img src="diagrams/tap.jpg" width="300" height="120"> </p><BR>

Now place a tap cell on both ends of the row of cells to provide connections to VDD and VSS. Align them precisely.

**_Step 7: Hand Route the LFSR4 circuit_**

### Task 4 - Verification using Calibre_**

....
