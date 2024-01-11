# LayoutMesh
A CBUS module configuration utility for model railway layouts.

## Update
Alpha version will be released shortly.
<p><img src="Screenshots/20240110-InitUI.gif" width="1174"/>
<br>

Please note that this is an alpha (preview) release - the application has not undergone extensive testing in real-life environments; it has limited functionality in some areas; any feature may be modified or removed.

CANMIO and CANPAN modules are supported in terms of the data describing events and event actions, and also for module-specific node configuration screens.  Other module types are currently supported with a generic node and event variable editor.  Addition of module specific support should not require changes to the main application.

## Design

### Concept
**A consistent and coherent CBUS layout configuration utility that can be used with the minimum of background knowledge by layout builders, whilst also being a powerful tool for module development.**

LayoutMesh is an application which represents a model railway layout as a series of devices (e.g. a switch, a point motor, a signal) and a series of events (e.g. 'Select route A', 'Loco detected in block').  The user creates an event, giving it a name and adding free-text notes if required.  The event is then configured to communicate between devices by defining trigger and response actions; for example: a switch is assigned to trigger the event, and two point motors are assigned to respond to the event.

For module developers, LayoutMesh provides CBUS message display and logging, manual triggering of events, and firmware uploading. Data files, which contain all device, node, module, processor, and event information, are human-readable. Data loading is edit-tolerant, with the ability to handle missing and duplicated data in a predictable manner.

### Design Specifics
* Modules represented by nodes (specific instances of a module), and devices (multi-channel nodes have a device per channel).
* All main entities (devices, nodes, modules, processors & events) may be given a meaningful name, have free-text notes added.  Each entity type has its own hierarchical tree structure, allowing entities to be grouped together however the user wishes.
* Events may have multiple trigger actions and multiple response actions. Trigger and responder devices may be on different nodes, or the same node  (firmware permitting).
* Capture and display of CBUS traffic in real time, including data translation.  The CBUS display is a separate window, allowing it to be positioned and accessed independent of the main application.
* A 'synchronise' display handles hardware or data changes by comparing the hardware and data configuration of nodes, and, if they don't match, allowing the user to choose synchronisation options such as making the hardware match the data, or vice versa.
* Data and hardware configuration information is backed up before any changes are made, and can be restored easily if required.
* No module-specific code in the main application. Node configuration may be provided for those modules that don't have a self-configuration mechanism, using separately installable 'plug-in' files.
* Simple installation from a .zip file. No hidden directories or files.
* All code and documentation will be made available on GitHub under an open MIT license.

### Target Systems
The application is written in C# for Windows systems, based on .NET Desktop 8.

## Installation
TBA

## Getting Started
There are two ways of getting started with LayoutMesh:
1. Importing data from the MERG FlimConfig utility (FCU).
1. Connecting LayoutMesh to a CBUS network, and reading the data from each node.

### Importing Data from a FlimConfig File
This process imports nodes and events from a FlimConfig data file.  It can be done without being connected to the CBUS network, so is a safe way of exploring LayoutMesh without the danger of inadvertently making configuration changes.
* Run LayoutMesh.
* It is probably a bad idea to import data over any existing data, so select 'File' -> 'New' to clear all data and start afresh.
* Select 'File' -> 'Import FCU Config File...'. Navigate to the FlimConfig data directory (usually %MyDocuments%\flimConfig\Configs), then double-click the required layout file (e.g. Layout1.xml).
* There may be a display showing new devices and events that have been added to the MeshData data.
* Select 'File' -> 'Save As...', and save the data as a MeshData layout file.

### Reading data from CBUS Nodes
This process reads configuration data directly from CBUS nodes.  It is a good way of "starting from scratch".  It is not possible to run LayoutMesh and FlimConfig at the same time unless you have two CBUS adapters (e.g. 2x CANUSBs) on your network.
* Run LayoutMesh.
* Click on the CBUS window.  If you can't see the CBUS window at any time, click on the 'Show CBUS Window' button on the main application window.
* Select 'CBUS' -> 'COM Port', then select the appropriate COM port.  If the CBUS is active, a green indicator should appear in the bottom left-hand corner ot the CBUS window; adjacent amber indicators show messages being received and transmitted.
* Select 'MeshData' -> 'Synchronise...', or click the 'Synchronise MeshData and Hardware' button.  A new window (the Sync window) will open, showing all the nodes on the CBUS network, with a 'Response' of 'New Node'.
* For each node, Click 'Options'.  In the options dialog window select the first entry 'Add a new MeshData node with node number <nn>', then click 'OK'.
* There may be a display showing new devices and events that have been added to the MeshData data.  The node's 'Response' should be showing as 'Synchronised', indicating that node parameters and configuration data in MeshData match that of the hardware.
* Close the Sync window.
* Select 'File' -> 'Save As...', and save the data as a MeshData layout file.

### Notes
To avoid inadvertently making live configuration changes, you can select 'CBUS' -> 'Write Lock' in the CBUS window.  With the Write Lock activated, any LayoutMesh operation that writes configuration changes to a CBUS node is disabled.
