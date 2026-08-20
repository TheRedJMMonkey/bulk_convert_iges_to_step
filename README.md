# Description

This is a FreeCAD macro written to quickly convert one or more IGES files into STEP model files for use in other CAD systems or downstream workflows.

## Usage

This macro opens a file selection dialog and allows you to choose one or more IGES files to convert.

When you run the macro, it will:

* Open the selected IGES files one at a time
* Create a folder called `stepout/` in the same directory as the source file if it does not already exist
* Shift the imported geometry so that its bounding box is aligned to the origin
* Export the result as a STEP file in the `stepout/` folder
* Close the document after each conversion

The exported STEP file will be named the same as the source file, with the `.step` extension.

## Installation

1. Download the [bulk_convert_iges_to_step.FCMacro](bulk_convert_iges_to_step.FCMacro) FreeCAD macro (right-click the link and select "Save Link As") to your User Macro Location (`%APPDATA%/FreeCAD/Macro/` by default on Windows)

## Behavior

The macro will loop through each selected IGES file and perform the following procedure for each one:

* Check that a valid IGES file was selected
* Create a `stepout/` folder in the same directory as the source file if needed
* Close the current document if another one is open
* Import the IGES file into a new FreeCAD document
* Compute the combined bounding box of all imported objects
* Translate the geometry so that the minimum X, Y, and Z values are moved to the origin
* Group the shifted objects together
* Export the grouped object as a STEP file
* Close the document after the export is complete
