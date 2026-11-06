# Description

This is a FreeCAD macro written to quickly convert one or more IGES files into STEP model files for use in other CAD systems or downstream workflows.

## Usage

This macro opens a file selection dialog and allows you to choose one or more IGES files to convert.

When you run the macro, it will:

* Read the selected IGES files one at a time
* Create a folder called `stepout/` in the same directory as the source file if it does not already exist
* Shift the imported geometry so that its bounding box is aligned to the origin
* Export the result as a flat STEP file (no nested assembly) in the `stepout/` folder

The exported STEP file will be named the same as the source file, with the `.step` extension.

The macro works on shapes directly and never creates a FreeCAD document, so any document you already have open is left untouched.

## Installation

1. Download the [bulk_convert_iges_to_step.FCMacro](https://raw.githubusercontent.com/TheRedJMMonkey/bulk_convert_iges_to_step/main/bulk_convert_iges_to_step.FCMacro) FreeCAD macro (right-click the link and select "Save Link As") to your User Macro Location (`%APPDATA%/FreeCAD/Macro/` by default on Windows)

## Behavior

The macro will loop through each selected IGES file and perform the following procedure for each one:

* Create a `stepout/` folder in the same directory as the source file if needed
* Read the IGES file into a single shape (files with no geometry are reported and skipped)
* Compute the exact bounding box of the shape
* Move the geometry so that the minimum X, Y, and Z values are at the origin, rewriting the geometry itself rather than setting a placement
* Export the shape as a STEP file

If a file fails to convert, the macro prints the error and continues with the remaining files. A summary of converted and failed files is printed at the end.

### Why the shift is baked into the geometry

Applying the shift as an object placement, or exporting the shape inside a group, makes FreeCAD write the STEP file as a nested assembly: a parent product containing the part, plus an offset transformation. Some importers, such as Bend-Tech DragonCAM, can fail to read these files. Rewriting the geometry directly produces a single part with its coordinates already at the origin, so there is no transformation for the importer to apply.

The exact bounding box is used instead of `Shape.BoundBox`, which is padded by the shape tolerance and would leave the part about 0.001 mm away from the origin.

### Notes

* The STEP product name is generic (it is not the file name) and no color styling is written.
