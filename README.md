# Sysmon View

Sysmon View helps in tracking and visualizing Sysmon logs

**Getting Started**

To get started, we need to export Sysmon events to XML file using WEVTUtil, this file will be imported later by Sysmon View:

`WEVTUtil query-events “Microsoft-Windows-Sysmon/Operational” /format:xml /e:sysmonview > eventlog.xml`

Once exported, run Sysmon View and import the generated file “eventlog.xml” (or the name you selected), please note that this might take some time, depending on the size of the log file (this needs to be done once per log file, subsequent runs of Sysmon View do not require importing the data again, just use the command `File -> Load existing data` to load previous data and work with it again)

All data will be exported to a database file _(SQLite)_ named **SysmonViewDB** that resides in the same location as Sysmon View. This file can be shared with others, just place the file in the same location as  Sysmon View and use the command `File -> Load existing data`

# Sysmon Shell

Sysmon Shell can aid in writing and applying Sysmon XML configuration through a simple GUI interface, it can also be used to learn more about Sysmon configuration options available with each release, in a nutshell:

* Sysmon Shell can load Sysmon XML files configurations: with version 1.0, I am only supporting the latest schema v3.30 for Sysmon v6.01 and above, future updates to Sysmon will be supported. In addition, the tool won’t be loading any configuration of Sysmon from the registry, however, I might add support to this feature in the future.
* It can export/save the final XML to a file.
* It can apply the generated XML file by calling Sysmon.exe -c directly (creating a temp XML file in the same folder where Sysmon is installed), for this reason, it will need elevated privileges (the need for this is inherited from Sysmon), the output of applying the configuration will be displayed in the preview pan (Sysmon output)
* XML Configuration can be previewed before saving in the preview pane
* If you are using Sysmon for malware analysis, you might find the last tap marked "Logs Export" useful, as it allows exporting Sysmon logs to XML file for use later, for example, I use it in Sysmon View for later analysis, the export has 3 options:
    * Export only
    * Export and clear Sysmon event log
    * Export, backup evtx file and clear the event log
* The utility contains descriptions for all events types taken from Sysmon Sysinternals home page (https://technet.microsoft.com/en-us/sysinternals/sysmon)

**What it won’t do**: warn you about Include/Exclude conflicts or attempt to validate the rules itself, however, once configuration is applied, the preview pane will display the output from Sysmon to show the results (this is the output of Sysmon -c command), from which errors can be identified

# Notes
* Password of archives is **password**
* All executables has been **digitally signed**
* All executables are **packed to reduce their size** (all dependencies has been statically linked)

# Additional Resources
* You can read more about Sysmon View & Sysmon Shell on my website https://nosecurecode.com/
* For an overview on Sysmon and Sysmon View in action, check out https://www.fwhibbit.es/sysmon-the-big-brother-of-windows-and-the-super-sysmonview