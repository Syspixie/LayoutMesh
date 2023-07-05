# LayoutMesh
A CBUS module configuration utility for model railway layouts.

## Design

### Vision
**To create a CBUS module configuration utility that can be used with the minimum of background knowledge by
layout builders, whilst also being a powerful tool for module development.**

LayoutMesh will be an application which represents a layout as a series of devices (e.g. a switch, a point motor, a signal) and a series of events (e.g. 'Select route A', 'Loco detected in block').  The user creates an event, giving it a name and adding free-text notes if required.  The event is then configured to communicate between devices by configuring trigger and response actions; for example: a switch is assigned to trigger the event, and two point motors are assigned to respond to the event.

For module developers, LayoutMesh will provide CBUS message display and logging, manual triggering of events, and firmware uploading. Data files, which contain all device, node, module, processor, and event information, will be human-readable. Data loading will be edit-tolerant, with the ability to handle missing and duplicated data in a predictable manner.

### Design Specifics
* No module-specific code in the main application.
* Simple installation: either using a package .msi or .exe file, or a .zip file.
* Modules represented by nodes (specific instances of a module), and devices (multi-channel nodes have a device per channel).
* All main entities (devices, nodes, modules, processors & events) may be given a meaningful name, have free-text notes added, and may be collected together in a named group.
* Events may have multiple trigger actions and multiple response actions. Trigger and responder devices may be on different nodes, or the same node  (firmware permitting).
* A 'synchronise' mechanism to handle hardware or data that has changed outside of the application.
* Capture and display of CBUS traffic in real time, including data translation.
* Data backups made before major changes saved to hardware or data.
* Node configuration provided for those modules that don't have a self-configuration mechanism, using a separately installable 'plug-in' package.

### Target Systems
The application is written in C# for Windows systems, based on .NET Desktop 7.

## Progress
Project started January 2023.

As of July 2023...

#### Features Complete or Near Completion (subject to testing)
* 'MeshData' files - XML file load and save; JavaScript parse and execute.
* Main user interface - entity tree views; central entity information display panel; CBUS message display; drag & drop.
* CBUS interface - COM port; message transmit and receive; formatting of CBUS data; logging.
* Editing entity names, notes, and groups.
* Editing event actions - assigning trigger and response actions; deleting actions.

#### In Progress
* Synchronisation between MeshData and hardware.
* Adding context menus for entity add/edit/delete.

#### Not Started
* Module-specific node configuration plug-ins.
* Firmware upload.
* Testing. And more testing.
* Documentation.

#### Not Known
* Interaction with SLiM nodes.
* Interaction with DCC nodes.
* There appears to be no documentation for the current FCU's functionality, so there are possibly things in there that I (and probably most people) know nothing about.

### Screenshots

User interface: entity treeviews on left and right of central information panel, and CBUS message panel.
<br><img src="screenshots/20230705-InitUI.gif" width="850"/>

User interface with event "Event 16" selected: central panel showing data available about the event.
<br><img src="screenshots/20230705-UIExample.gif" width="850"/>

Event Configure->Actions shows the Event actions dialog: note level of detail about each action.
<br><img src="screenshots/20230705-Actions1.gif" width="732"/>

Add action dialog page 1: device "MIO Test channel 2" selected as a response action.
<br><img src="screenshots/20230705-AddAction1.gif" width="605"/>

Add action dialog page 2: specific action (flash output) selected.
<br><img src="screenshots/20230705-AddAction2.gif" width="605"/>

Event actions dialog: showing the newly added action.
<br><img src="screenshots/20230705-Actions2.gif" width="732"/>

## Notes

Looked at JSON as a format for MeshData files; found it difficult to manually read and edit.  Reverted to trusty old XML - tags let you know what you are looking at. By careful association of data model classes with the underlying XML, the application preserves most user formatting, comments etc. when saving the XML file.

Pleased with the performance of CBUS message capture, display, and logging. Appears to be limited by the speed of the CAN bus, not the application itself. Certainly a lot faster than the current FCU - for example: FCU takes 4.7 seconds to read 127 node variables from a CANMIO node; LayoutMesh reads the same node variables, and the events, and the event variables, and runs JavaScript to translate the event variables into actions ... all in 0.75 seconds.
