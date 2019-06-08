# Sysmon Tools
### Utilities for Sysmon

This repository contains the following:

-   **Sysmon View:** an off-line Sysmon log visualization tool.
-   **Sysmon Shell:** a Sysmon configuration utility.

# Content

-   Release Notes
-   Sysmon View
-   Sysmon Shell
-   Additional Resources
-   License

# Release Notes

-   Sysmon View: Version 3.0 can build a tree view for process creation hierarchy
-   Sysmon View: Enhanced searching (all events data can be searched)
-   Sysmon Shell: Added a command to upgrade configuration files to V9.0 (temporary solution)

# Sysmon View

Sysmon View helps in tracking and visualizing Sysmon logs by logically grouping and correlating the various Sysmon events together, using existing events data, such as executables names, session GUIDs, event creation time, etc., the tool then re-arranges this data for display into multiple views

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/1.png "Sysmon View")

**Getting Started**

To get started, export Sysmon events to XML file using the built-in WEVTUtil, this file will be imported later by Sysmon View:

`WEVTUtil query-events "Microsoft-Windows-Sysmon/Operational" /format:xml /e:sysmonview > eventlog.xml`

Once exported, run Sysmon View and import the generated file “eventlog.xml” (or the name you selected), please note that this might take some time, depending on the size of the log file (the file needs to be imported once, subsequent runs of Sysmon View do not require importing the data again, just use the file menu `File -> Load existing data` to load previously imported data again).

All data will be imported to a _SQLite_ database file named **SysmonViewDB** that resides in the same location as Sysmon View executable. This file can be shared with others if required, just place the file in the same location as  Sysmon View and use the command `File -> Load existing data`.

Each time a new XML file is imported, the database file will be deleted and re-created. To preserve any previously imported data, copy the database file to another location or simply rename it.

The database can be used directly in your own applications too, the database contains summaries of hashes, executables, IP addresses, geo mappings and all are logically linked through a file name or a session (executable GUID).

_You can query the database file directly using any SQLite management software without the need for Sysmon View, for example, to generate reports or analyze data_

**Sysmon Views**

**Process View** this view simply helps focus on a summary of "run sessions", for example, the analyst can start with executable name (such as cmd.exe) or event type (such as Network event), from there, further filtering can be applied, for example, finding running sessions originating for the same binary, but from different locations. This view utilizes the process GUID to filter events per session "run", selecting any running session (from the list of GUIDs) will show all other related (correlated) events in a simple data-flow-like view, sorted using the time of the event.
_Note: in case data is being imported from an Elasticsearch instance instead of single machine, events can be arranged per executable per machine - check previous section **"Experimental - Sysmon View and Elasticsearch"**)._

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/1_5.png "Sysmon View")

Access to Sysmon event details is provided by simply double-clicking any event in the view, for example, the previous screen capture shows the details of the _Process Creation_ event (event ID 1), the tool also can integrate with VirusTotal upon demand for further hash and IP lookup (Needs an API key registration).

**Map View** : During the events import process, there is an option to geo-locate IP addresses, if set, Sysmon View will try to geo-map **Network Destinations** using https://ipstack.com/ service.

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/2.png "Sysmon View")

In **map view**, it is easy to navigate between correlated (related) events by using a _network event_ as a starting point, again, the tool is able to achieve this using the running process session GUID. To explore related events, use the hyperlinks for the session GUID, a new view similar to **process view** will show up in a new window with all related session events:

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/2_5.png "Sysmon View")

**All Events View** can also be used to do a _full search_ through all Sysmon collected events data, it also helps in viewing events that do not relate to other events, such as the "Driver Loaded" event type. Navigation between related events is still provided using the process GUID in addition to event details by clicking on _FID link_

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/3.png "Sysmon View")

Additionally, The **All Events View** supports pivot-like (grouping) arrangement of events, by machine name, event type or GUID, as shown below

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/4.png "Sysmon View")

Multiple grouping levels are also possible

![Sysmon View](https://nosecurecode.com/wp-content/uploads/2018/07/5.png "Sysmon View")

# Sysmon Shell

Sysmon Shell can aid in writing and applying Sysmon XML configurations through a simple GUI interface.

![Sysmon Shell](https://nosecurecode.com/wp-content/uploads/2017/11/HeadImageSysmonShell.png "Sysmon Shell")

Sysmon Shell can also be used to explore the various configuration options available to Sysmon, easily apply and update XML configuration, in addition to exporting Sysmon events logs, in a nutshell:

* Sysmon Shell can load Sysmon XML configuration files: current version supports all Sysmon schemas. (the tool doesn't load any configurations directly from registry, only from XML files).
* It can export/save the final XML to a file.
* It can apply the generated XML configuration file by calling `Sysmon.exe -c command` directly (creating a temporary XML file in the same folder where Sysmon is installed), for this reason, if this feature is used, Sysmon Shell will require elevated privileges (the need for this is inherited from Sysmon process itself), the output of applying the configuration will be displayed in the preview pan (this is Sysmon generated output)
* XML Configuration can be previewed before saving in the preview pane
* The last tab (marked "Logs Export") might become handy to quickly export Sysmon event logs to XML, which can be later used with **"Sysmon View"** for events analysis, the export has three options:
    * Export only
    * Export and clear Sysmon event log
    * Export, backup _evtx_ file and clear the event log
* The utility has descriptions for all events types taken from Sysmon Sysinternals home page (https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
* Sysmon Shell comes _bundled_ with many Sysmon configuration templates created by other security professionals

![Sysmon Shell Templates](https://nosecurecode.com/wp-content/uploads/2017/12/SysmonShellTemplates.png "Sysmon Shell Templates")

**What it won’t do**: warn you about Include/Exclude conflicts or attempt to validate the rules itself, however, once the configuration is applied, the preview pane will display the output captured from Sysmon.exe when configuration is applied (the output of `Sysmon -c command`), from which errors can be identified

# Additional Resources

* You can read more about Sysmon View & Sysmon Shell on my blog https://nosecurecode.com/
* The following is a good overview about Sysmon and Sysmon View https://www.fwhibbit.es/sysmon-the-big-brother-of-windows-and-the-super-sysmonview

# License

```
Copyright 2018 Nader Shalabi. All rights reserved. 

Redistribution and use in source and binary forms, with or without modification, are permitted provided
that the following conditions are met: 
1. Redistributions of source code must retain the above copyright notice, this list of conditions and
   the following disclaimer. 
2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions
   and the following disclaimer in the documentation and/or other materials provided with the 
   distribution. 

THIS SOFTWARE IS PROVIDED BY NADER SHALABI ''AS IS'' AND ANY EXPRESS OR IMPLIED
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL NADER SHALABI
OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

The views and conclusions contained in the software and documentation are those of the authors and
should not be interpreted as representing official policies, either expressed or implied, of Nader Shalabi.
```
