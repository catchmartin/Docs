# Laser Cutting Guide for DCU
## Operating Laser Cutter
* Power on laser cutter at wall
* Power on at side of laser cutter
* Let laser move about and then stop
* Turn XY off (button on cutter)
* Lay down your sheet on top of some spacer discs
  * This raises it off metal bed
* Move carriage until laser at top-left corner of sheet
  * Try not to have spacer discs underneath cuts/engraving points because they'll melt on
* Raise/lower bed to get z axis guide (swings down) just at right distance from sheet (using direction buttons on cutter)
* Close lid and hit "go" to lock chassis again
* Turn on pump on wall behind cutter
* Open dxf file in CorelDraw
  * Select all
  * In Object properties pane on right, pen nib, 0.5pt
    * This rasterizes all vectors
  * Select each of the cuts in turn
    * In object properties make hairline
  * To fill shapes, do that now
  * Copy, paste, and use arrow buttons to tile as required
  * To cut, click on File -> Print
    * Select the laser on either right/left
    * Then click preferences
      * Advanced, (Select material)1.5mm pmma
      * Change speed on raster per square guide in drawer behind you
        * Keyrings were cut at 100% speed
      * Vector power setting, up by 2
      * Make sure colors configure right for cutting, engraving, etc.
        * Default seemed to be red=cut, blue=engrave
      * Click ok
    * Click print
* Turn on big exhaust on for relevant laser cutter
* On cutter hit go to start print queue
* After all cutting done, turn off:
  * Big exhaust
  * Small fan on wall
  * Laser cutter on machine followed by wall

## Sourcing Materials

### Acrylic sheets, 2mm should be fine for anything really
* goldstarplasticsireland.ie or alperton.ie for sheets cut to 300 X 400 mm

### Wood sheets
* woodworkers.ie looking for water resistant wood

## Design Helpers

### Using Inkscape and exporting to .dxf
* http://wiki.imal.org/howto/inkscape-and-lasercutter
  * Set units to mm for inkscape and document
  * Export using EPS and pstoedit method, not dxf plugin
    * Means the text in the SVG file can still be edited later having not been converted to a path
    * Also means you don't have to ungroup etc.
    * use "-ctl" option for putting different colors on different layers

### Box Design
* makercase.com to make up enclosure designs
  * Great starting point for any enclosure
