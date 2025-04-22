

## Converting a 3D  CAD File to .STL and slicing it.
---

1. **Converting the 3D  CAD File to .STL Format**
   Understanding the CAD File Format
   The initial file format provided is a CAD file, which might be in formats such as
   .DWG, .IGES, .STEP, or .3MF. These formats need to be converted to .STL for
   compatibility with 3D printing workflows.
   
  1. **Open the CAD File:**
     - Use CAD software such as AutoCAD, Fusion 360, SolidWorks, or any other
     - compatible application.
     - Ensure the 3D  model is fully loaded and verified for correctness
     - (dimensions, structure, and any text/engraving).

  2. **Export as .STL:**
     - Navigate to the export or save-as menu.
     - Select the .STL file format.
     - Adjust export settings:
     - Set resolution to "High" (if applicable). 
     - Use ASCII or Binary encoding (Binary is smaller in size).

  3. **Ensure units match the design dimensions (e.g., millimeters or inches).**

  4. **Save the File:**
   Name the file appropriately (e.g., Leopard2A7.stl).
   Save it to a location accessible for the next steps.


2. **Preparing the File in FlashPrint Software**

   1. Open FlashPrint.
   2. Import the `.stl` file by clicking on load file or drag and drop the model.
   3. Verify that the 3D dice model appears correctly in the build area.
    ![[Pasted image 20250129214103.png]]

3. **Adjusting Model Settings**

   1. **Positioning the Model**:
    - Use the "Move" tool to position the dice in the center of the build plate.
    - Ensure the model is flat on the build surface.
     ![[Pasted image 20250129214420.png]]
     
   1. **Rotating:**
    - Adjust the orientation using the "Rotate" tool for optimal printing 
	- Make sure to position the flat part of the model as base so that it requires less support material.
     ![[Pasted image 20250129214914.png]]
     
   1. **Scaling**:
    - Verify the dimensions of the model.
    - Use the "Scale" tool if resizing is required
     ![[Pasted image 20250129215407.png]]
       
   1.  **Cutting:**
    - This step is performed if we require to print parts of model separately.
    - Since we are printing in single piece we won't need to perform cutting.
     ![[Pasted image 20250129220136.png]]
     
   1. **Duplicating**
     - If you want to print multiple models at once we can choose to duplicate the model.
     ![[Pasted image 20250129220458.png]]
        
   1. **Generating supports**:
    - Click on `support` tab 
    - Select the supports type from two available options.
    - You can layout support manually or just click on auto supports.
     ![[Pasted image 20250129223322.png]]
     

4. **Slicing Settings**
   1. Click on the "Start Slicing" button.
    ![[Pasted image 20250129224017.png]]
    
   1. Fill the printer details 
     ![[Pasted image 20250129224119.png]]
     
   1. Set the slicing parameters:
    - Layer Height: Choose a value (e.g., 0.1mm for fine detail, 0.2mm for standard).
      ![[Pasted image 20250129224304.png]]
      
    - Infill Density: Adjust based on strength needs (e.g., 20% for standard dice).
     ![[Pasted image 20250129224258.png]]
     
    - Support Structures: Enable if the model design has overhangs.
      ![[Pasted image 20250129224355.png]]
      
    - Raft Parameters: Modify or leave the default values for raft layer as needed.
        ![[Pasted image 20250129225424.png]]
        
    - Print Speed: Use default or adjust as needed. 
      ![[Pasted image 20250129224824.png]]


5. **Preview the slicing**:
    - Review the slicing preview to ensure there are no errors in the layers.
    - Adjust settings if needed.
      ![[Pasted image 20250129225528.png]]
      
    - With the current parameters as it is, it takes 5 hours 28 minutes for the model to complete printing.
      ![[Pasted image 20250129225559.png]]
      
    - We can tweak with parameters to reduce the print time
       ![[Pasted image 20250129225820.png]]
       
     - In this case I increased the base print speed from 80% to 200% which reduced the time from 5 hours 28 mins to 4 hours 35 mins.


6. **Print the model**
   -  Once we finalize the preview with all the settings we can start printing the model by connecting to the device.
     ![[Pasted image 20250129230525.png]]
     
   - We can also save and download this file as `.gx` extension so that we can print it by sharing this file to printer via USB.
     ![[Pasted image 20250129230515.png]]

   

 

   



