# ImageSideGSTuning
Image-side Grayscale Adjustment Tool for Laser Marking

Lightburn currently (2023-05-28) maps gray values to power linearly. If black gets 100% power and white gets 0% power, then middle gray gets 50% power. This doesn't map well to different lasers, material types, or the logarithmic nature of how our eyes perceive contrast.

This is my attempt at offering a solution by preprocessing images in order to create the intended outcome. The image preprocessing route is much more ham-fisted than doing the adjustments to a laser's power curve, but Lightburn doesn't currently provide plugin support.

I'm not a programmer by trade, I just happen to know enough things about lasers and image processing to slap this together. Please take what I've done and do it better. :) This project is MIT licensed, so feel free to directly take the concepts herein and deploy it in a more meaningful way. I'd appreciate some sort of attribution, but it's not required.

If you're feeling inspired, feel free to [buy me a coffee](https://ko-fi.com/aaddrick)

## Todo List
This is roughly in order, but not definitively. Mainly want to ensure the functionality is all listed.
- [x] Create github account and project
- [ ] Add button for selecting an image
- [ ] Figure out how to create an image preview pane which doesn't move as you scroll, or some other functionality to keep it user friendly.
- [ ] Add Grayscale conversion step if image is not already grayscale. 
  - Should I have an option to choose between linear and weighted grayscale?
- [ ] Add text box for entering number of knots
  - These will be used for manually adjusting the Catmull-Rom spline which will define the grayscale correction.
  - Default to 14 to give 16 points, including the control points at each end.
  - Maybe just leave it unchangeable to start, as I don't want to figure out the exception handling for gray values not being divisible by adjustment points.
- [ ] Add text box for entering Minimum Power 
- [ ] Add text box for entering Maximum Power
- [ ] Add button somewhere to generate test images at a given DPI for calibrating min and max power to assist with finding those values.
  - Images will use the lowest knot value and bottom control point for Max Power and likewise the highest knot value and top control point for Min Power
- [ ] Create a chart which shows the original image histogram in gray behind the new histogram in some other color which presents well.
- [ ] Create a chart which shows the adjustment curve with the 14 knots.
  - User should be able to click on the different knots and drag them along the vertical axis.
- [ ] Create some sort of output table which shows the gray values of the 16 points and the current "power level"
  - Initialize as follows:
  - Rows = Knots + 2
  - n = row number, indexed at 0
  - Row<sub>0</sub>: GV = 0, Power = max power
  - Row<sub>n</sub>: GV = 255, Power =  min power
  - Row<sub>1->n-1</sub>: GV =  $\dfrac{255}{Knots+1} * n$, Power = $max power - (\dfrac{max power - min power}{Knots + 1} * n)$
- [ ] Allow editing of the output table or chart to update one another appropriately
- [ ] Create button to save all settings in order to reload later on
  - Not sure whether to use XML or JSON yet, probably XML.
- [ ] Create button and function to load settings from the saved file.
- [ ] Add relevant exception handling...
- [ ] Create button to process image (this is very roughed in. need to test)
  - For each of the GV's between 0 and 255 (1 and 254):
  - Calculate deviation as a percentage at that position between linear curve and final Catmull-Rom curve.
  - Map each old GV to a new GV
  - Iterate through all pixels in the image and replace old GV's with new GV's.
  - Display new processed image
- [ ] Create an area to display the new image
- [ ] Create a button to save the new image

## Longer Term Todo list..
- [ ] Add ability to automatically grayscale tune using exponential measurements (Density) or linear measurements (L*)
  - Relying on nearest neighbor interpolation
