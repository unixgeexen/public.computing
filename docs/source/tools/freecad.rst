FreeCAD Drawing
---------------

Introduction
============

-  Using `FreeCAD <#FreeCAD>`__ a parametric drawing design tool
-  `Product Wiki <https://www.freecadweb.org/wiki/>`__
-  `Product Manual <https://www.freecadweb.org/wiki/Manual:Introduction>`__
-  `External workbenches information <https://www.freecadweb.org/wiki/External_workbenches>`__ and `Addons project <https://github.com/FreeCAD/FreeCAD-addons>`__
-  Primarily a 3D modelling application, 2D tools available but not as advanced as other tools

Questions and research
======================

-  Is it a good idea to build up components for reuse
-  Do you want components for multiple projects or specific to a given type
-  How to use part design workbench - tutorial
-  How do you handle moving parts - constraints - e.g. bike/person interaction

Concepts
===========
- FreeCAD modules - every App object has a Gui object
   - App - access object defining properties - vector
      - e.g. width, length, height
      - contains all code to run in console mode (no GUI)
   - Gui - control presentation of objects on the screen
      - e.g. face colour, drawing mode
   - Part
      - Geometry - building block of topological objects e.g. line, circle
         - makeLine((0,0,0),(10,0,0))
         - line=Part.LineSegment(vec1,vec2)
      - Topology (Shape) - wire, edge, vertex
         - edge=line.toShape()
      - Shape Modification
         - Translate (translate) - move from one place to another
         - Rotate (rotate) - rotate by an angle around a rotation point or axis
         - Matrix - use matrices to translate, rotate and/or scale an object
         - Scale (scale) - scale can be done non-uniformly
         - Subtraction (cut) - remove one shape from another
         - Intersection (common) - the intersection between two shapes
         - Union (fuse) - combination of two shapes
         - Section (section) - intersection between a solid and a plane
         - Extrusion (extrude) - pushing a flat shape into a solid
      - Selection (getSelection) - iterate over objects and their subelements

- Objects
   - Document - contains geometry and can be saved to a file

Workbenches
===========

- Sketcher for working with geometry-constrained sketches.
   -  Normally used to create the sketch and then processed with Part Design to extrude and create a basic solid
   -  Build/edit complex 2D objects (sketches)
   -  Precise positioning and relationships with contraints
- Part Design for building Part shapes from sketches - parametric, feature editing methodology
   -  sequentially transform basic solid
   -  advanced tools to build solid parts
   -  has same tools as Sketcher
   -  it can only produce solid parts
- Part for working with CAD parts - uses CSG (constuctive solid geometry) methodology.
   -  solid parts - cube and sphere primitives
   -  simple geometric and boolean operations
   -  foundation of freecad geometry system
   -  most other workbenches produce part-based geometry
- Spreadsheet for creating and manipulating spreadsheet data. - can be referred to from within `FreeCAD <#FreeCAD>`__ allowing use as master data structures
   - see alias manager for setting aliases
   - create spreadsheet in libre calc, save as csv, import to freecad - seems to have limited spreadsheet editing facilities within freecad
   - cells can have a name which can be used in any field supported by the expressions engine
      - name by RC cell/Properties/Alias
   - whilst limited in functionality it has the basics of formulas
   - spreadsheet can retrieve values from properties of objects within the document
      - properties are in the property editor window in the data tab
   - cannot have circular dependencies - e.g. object dependent on a spreadsheet and another part of the spreadsheet dependent on the same object
      - must create two spreadsheets to get around this
   - can set values of objects - select object/select property value cell/select expression editor/reference spreadsheet value (`InternalSpreadsheetName <#InternalSpreadsheetName>`__.\ `CellName <#CellName>`__)
   - can be referenced by Python scripts
   - Review expressions engine
- FEM provides Finite Element Analysis (FEA) workflow.
- Mesh for working with polygonal meshes. - convert to and from Part
- TechDraw is the more advanced and feature-rich successor of Drawing
- Drawing was used for displaying your 3D work on a 2D sheet but has now been deprecated, it is still needed to read old `FreeCAD <#FreeCAD>`__ files that contain a Drawing object originally made with this workbench. See `TechDraw <#TechDraw>`__ Workbench, which is a more advanced replacement.
- Creation and manipulation of 2D drawing sheets
- Display 2D views of 3D work
- Export to SVG, DXF, PDF for printing etc
- Draft contains 2D tools and basic 2D and 3D CAD operations.
   -  basic 2D CAD drafting tasks - lines circles
   -  actions lie move, rotate, scal
   -  drawing tools like grid and snapping
   -  principally meant to draw the guidelines for Arch objects
   -  `FreeCAD <#FreeCAD>`__'s swiss army knive
   -  Can convert draft objects to sketches and then used in part design
- Start Center allows you to quickly jump to one of the most common workbenches.
- Surface workbench provides tools to create and modify surfaces. It is similar as the Part Shape builder Face from edges.
- Test Framework is for debugging `FreeCAD <#FreeCAD>`__.
- Web provides you with a browser window instead of the 3D-View within `FreeCAD <#FreeCAD>`__.
- OpenSCAD for interoperability with `OpenSCAD <#OpenSCAD>`__ and repairing Constructive Solid Geometry (CSG) model history.
- Path is used to produce `G-Code <#G-Code>`__ instructions. It is still in a stage of development.
- Plot is used to edit and save output plots created from other modules and tools.
- Points for working with point clouds.
- Raytracing for working with ray-tracing (rendering)
- Reverse Engineering is intended to provide specific tools to convert shapes/solids/meshes into parametric `FreeCAD-compatible <#FreeCAD-compatible>`__ features. (under development)
- Robot for studying robot movements.
- Ship `FreeCAD-Ship <#FreeCAD-Ship>`__ works over Ship entities, that must be created on top of provided geometry.
- Arch for working with architectural elements.
   -  Create BIM objects or give BIM attributes to obects from other workbenches and export them to IFC (Industrial Foundation Classes)
   -  BIM (Building information modeling) projects (civil engineering and architecture)
   -  Contains all tools from Draft workbench
- Image for working with bitmap images.
- Inspection is made to give you specific tools for examination of shapes. It is still in development.
- Some external workbenches `also see <https://github.com/FreeCAD/FreeCAD-addons>`__
   -  Drawing Dimensioning tools to work directly on Drawing Sheets and allow you to add dimensions, annotations and other technical symbols with great control over their aspect.
   -  Fasteners a wide range of ready-to-insert fasteners objects like screws, bolts, rods, washers and nuts. Many options and settings are available.
   -  Assembly2 tools to mount and work with assemblies.

Camper Trailer
==============

-  Use Input Spreadsheet
-  Probably have separate drawings for canvas and trailer - perhaps even separate drawings for trailer and camper parts
-  Use sketcher to build basic structure with constraints
-  For improved performance use geometric constraints
-  Try connect a sweep macro to build solids out of lines for framework
-  Then use part design if necessary

Python Bike
===========

- Use Input Spreadsheet
   -  Person measurements
   -  Fixed bike measurements
   -  Can you do angles in spreadsheets
- Create person drawing

Examples
========

- Tower frame `youtube <https://www.youtube.com/watch?v=Fil77xovYy0>`__
   -  new file
   -  sketcher workbench
   -  new sketch
   -  create legs - lines
   -  set constaints
   -  set distance between bottom poiints
   -  distance between top two points
   -  set angle to 80 degrees
   -  make diagonals with polyline
   -  set constraints
   -  close sketch
   -  select sketch set angle to -80degrees same as other angles
   -  use macro options - `macro connect and sweep <https://www.youtube.com/redirect?redir_token=TIK2YV7ZJb_O0PKHVmB_Zw8ApJh8MTU1NjUxMTM1OEAxNTU2NDI0OTU4&q=https%3A%2F%2Fwww.freecadweb.org%2Fwiki%2FMacro_Connect_And_Sweep&v=Fil77xovYy0&event=video_description>`__
   -  select two endpoints and create solid
