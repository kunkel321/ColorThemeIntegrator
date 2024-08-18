# Color Theme Integrator

![Screenshot of main window](https://i.imgur.com/BJaONaj.png))

Kunkel321: 8-16-2024
https://github.com/kunkel321/ColorThemeMaker
https://www.autohotkey.com/boards/viewtopic.php?f=83&t=132310

WhiteColorBlackGradient function is based on ColorGradient() by Lateralus138 and Teadrinker. 
Calling the Windows color picker is also based on Teadrinker code.  
I Used Claude.ai for debugging several parts and doing the "split complemplementary" math. 
Some aspects are also from https://colordesigner.io/color-wheel

It is a Color Theme "Integrator" because is writes the colors to an ini file, which several of my other AHK gui-based tools read from.  

The colorArray has 120 elements, equidistantly circling the color wheel.  I've attempted to create two color wheel options:
* RGB uses "additive/light-based" color gradients.
* CYM is uses "subtractive/pigment-based" color gradients.
Both arrays start at red, then loop around, back to red.

Using terminology from the above colordesigner site...

Given a reference color, its "Complementary" color will be directly across from it, on the opposite side of the color wheel.  If, instead of choosing the Complementary color, you choose the two colors that are on equal-and-opposite sides of the Complementary color, then the three points will comprise a "Split Complementary" color set.  The below tool uses the color selected in the combobox as the reference color and determines which color in the colorArray is its Complementary color.  The first up/down box in the gui (“Split Size”) defines the number of steps from the Complementary position, that the other two colors should be.  

The three theme color variables are used:
* "fontColor" is the color of the text on the gui and in applicable controls.  I.e myGui.SetFont("c" fontColor)
* "listColor" is the background color of the listView, and other applicable controls.  I.e. "BackgroundColor" as appears in the gui control options. 
* "formColor" is the GUI background color.  I.e. myGui.BackColor := formColor
The fourth color is "outside of" the theme:
* "warnColor" is the color used in place of the formColor, when the gui/app is in an alternate state. 

The colors are assigned by default as:  
* fontColor = The reference color that appears in the comboBox.
* formColor = The color that appears in the posing Counter-Clockwise from the Complementary position. 
* listColor = The color that appears in the posing Clockwise from the Complementary position. 
This means that the color which is chosen in the top comboBox corresponds to the font color of the form.  The gui element (font/list/form) which is used for the reference color (and which corresponds to the top comboBox) can be changed with the second comboBox "... Is Reference."   
The four color, warnColor, is not part of this color pattern scheme and is only changed via double-clicking the warnColor item on the bottom of the ListBox.  If you don't have any projects with guis that use alternate Gui Backcolors, then you can ignore the warnColor. 

So… If the Split Size is 10 or 15 or so, then the pattern will be “Split Complementary.”  If the Split Size is set to 0, then the listColor and the fontColor will be at the same position and will be "Complementary" to the formColor.   If the Split Size is the max of 60, then all three colors will be at the same location and the theme will be "Monochromatic."  If the Split Size value is around 45 to 50, then the pattern will be “Analogous” (again, see colordesigner site).  And if the Split Size is 20, then the pattern will be a “Triad.”   As indicated above, the primary color is set in the top comboBox, and the other two are the split colors.  The split colors can swapped by setting a negative number as the Split Size.

With a gui, the font needs to “stand out” from the background color of the gui and the controls, so the  colors chosen will often need to be adjusted in terms of lightness/darkness.  Typically the font will be on one end of the light/dark continuum, and the background colors will be at the other end.  The Light/Dark radios swap this.  The bottom three up/down boxes are for fine-tuning the light/darkness. Please note that the Light/Dark radios do not affect the warnColor.  It must be manually changed. 

Similar to changing the shading, users may wish to "tone-down" one or more of the colors by reducing the saturation.  This is simulated by fading the color to gray.  The saturation up/down controls are next to the shading ones. Double-clicking the "ReSaturate" text label will reset the saturation levels to max.  And double-clicking the "UnShade" text label will set the shade spinners to the middle number (neither light, nor dark.)

Near the bottom is the 4-row ListBox.  This shows the hexadecimal color values that will get exported.   The ListBox is also an "override."  If you can't get a color you like with the Hue, Shade, and Saturation controls, double-click the row in the list to pick a totally different color.  This will call a Windows color picker (based strongly on Teadrinker code) and disregard the above gui controls.  Note that if you then change a Hue, Shade, or Saturation controls, those controls will, again, take presidence and determine the colors used for the first three items (font, list, form). 

The current four colors, as well as the values of all the controls, can be saved to a configuration (.ini) file.  The tool will attempt to read from the file at start.  If the ini file is not found when the script starts, default values will be applied.  Near the top of the code is the "restartTheseScripts" list.  The tool will look for a list or scripts to restart (so that their gui colors will be updated.)  When the save button is pressed, the user will be given the option to also restart these scripts.  Uncheck the checkbox to bypass restarting the other scripts. 
