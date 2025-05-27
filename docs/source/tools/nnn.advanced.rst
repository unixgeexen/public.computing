=====================
nnn File Manager: Complete Visual Guide
=====================

.. contents:: Table of Contents
   :depth: 3


Installation Workflow
=====================
.. kroki::
   :type: plantuml
   
   @startuml
   participant User
   participant PackageManager
   participant System
   
   User -> PackageManager: sudo pacman -S nnn
   PackageManager -> System: Verify dependencies
   System --> PackageManager: Dependencies met
   PackageManager -> System: Install binaries
   System --> User: nnn v4.9 installed
   @enduml

File Operations Workflow
========================
.. kroki::
   :type: plantuml
   :caption: Batch File Processing

   @startuml
   start
   :Navigate to directory;
   :Select files with Space;
   repeat
    :Choose operation (Copy/Move/Delete);
    ->Error handling;
   repeat while (Errors?) is (Yes)
   ->No;
   :Confirm action;
   :Execute batch operation;
   stop
   @enduml

Context Navigation System
=========================
.. kroki::
   :type: blockdiag
   :caption: Multi-context Workspace Management
   
   blockdiag {
       orientation = portrait
       node_width = 150
       node_height = 60

     "Context 1" [label = "Context 1\n~/projects\n(3 files selected)", color = "#FFB3BA"];
     "Context 2" [label = "Context 2\n/var/log\n(0 selected)", color = "#BAFFC9"];
     "Context 3" [label = "Context 3\n/etc/nginx\n(1 selected)", color = "#BAE1FF"];
     "Context 4" [label = "Context 4\n/mnt/backup\n(8 selected)", color = "#FFFFBA"];

     "Context 1" -> "Context 2" [label = "Tab", style=dashed];
     "Context 2" -> "Context 3" [label = "Tab", style=dashed];
     "Context 3" -> "Context 4" [label = "Tab", style=dashed];
     "Context 4" -> "Context 1" [label = "Shift+Tab", style=dashed];
     
     "Context 1" -> "Context 3" [label = "cp $f1 $d3", color=blue];
     "Context 4" -> "Context 2" [label = "mv $f4 $d2", color=orange];
   }

.. list-table:: Context Management Features
   :widths: 30 70
   :header-rows: 1

   * - Feature
     - Description
   * - **Independent Workspaces**
     - Maintain separate:
       - Directory paths
       - Selection states
       - Navigation histories
   * - **Visual Identification**
     - Configure colors via environment variable:
       - Example: ``export NNN_COLORS="2136"``
   * - **Cross-context Commands**
     - Use special variables:
       - ``$d1-$d4``: Context directories
       - ``$f1-$f4``: Selected files




Plugin Architecture
===================
.. kroki::
   :type: graphviz
   
   digraph {
     rankdir=LR;
     node [shape=box];
     "nnn Core" -> "Plugin Manager" [label="Handles"];
     "Plugin Manager" -> "File Operations\n(e.g., fzcd, imgview)";
     "Plugin Manager" -> "Cloud Integration\n(e.g., rclone-mount)";
     "Plugin Manager" -> "Media Tools\n(e.g., mpvplay)";
     "User Config" -> "Plugin Manager" [label="NNN_PLUG"];
   }

Search Flowchart
================
.. kroki::
   :type: mermaid
   
   flowchart TD
     A[Start Search] --> B{Regex?}
     B -->|Yes| C[/Type pattern/]
     B -->|No| D[Instant filter]
     C --> E[Apply filter]
     D --> E
     E --> F{Results?}
     F -->|Yes| G[Select files]
     F -->|No| H[Adjust pattern]
     G --> I[Batch action]
     H --> C

Bookmark System
===============
.. kroki::
   :type: svgbob
   
   +------------------+
   |  User Presses B  |
   +------------------+
           |
           v
   +------------------+
   | Bookmark Manager |
   +------------------+
           |
           v
   +------------------+     +------------------+
   | Static Bookmarks |     | Runtime Marks    |
   | (NNN_BMS)        |<--> | (Comma key)      |
   +------------------+     +------------------+

Setup & Generation
==================
1. Install requirements:
.. code-block:: bash
   
   pip install sphinxcontrib-kroki

2. Add to ``conf.py``:
.. code-block:: python
   
   extensions = ['sphinxcontrib.kroki']

3. Build documentation:
.. code-block:: bash
   
   make html

References
==========
Built using Deepseek using the request: Review the following references and create an rst file covering the topics within them.  Create a table of the commands and their functions and include it within the rst file.  Create other tables as appropriate to document the information where it's suitable for table format.

- `Official Repository <https://github.com/jarun/nnn/wiki>`_
- `Arch Linux Guide <https://wiki.archlinux.org/title/Nnn>`_
- `Man Page <https://www.mankier.com/1/nnn>`_
- `Basic Use Cases <https://github.com/jarun/nnn/wiki/Basic-use-cases>`_
