# Sysmon View

Sysmon View helps in tracking and visualizing Sysmon logs by logically grouping and linking the various events generated using executable names, session GUIDs and sorted according the time of the event as shown in the following main view.

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/1.png "Sysmon View")

Previous view arranges Sysmon events per executable, it has many filtering options down to the executable path level and executable run sessions, and in case data is being imported from Elasticsearch instead of single machine, events can be grouped per executable per machine - check later the section **"Experimental - Sysmon View and Elasticsearch"**).

Sysmon View makes it simple and easy to get to the details of each events too.

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/1_5.png "Sysmon View")

The previous view shows the details of the _Process Creation_ events (event ID 1), the tool also integrates with VirusTotal for hash and IP lookup.

Sysmon View has several other views to display data, for example, the following is a geo-map view of **Network Destinations**

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/2.png "Sysmon View")

In this view, it is easy to move and relate the network event for example with the rest of the executable running session events (GUID), to do that, use the hyperlinks for the session GUID, here is the result:

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/2_5.png "Sysmon View")

Sysmon View can also be used to do a full search through all events data using the "All Events View" tab. This view provides the same events correlation and navigation functionality using the process GUID

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/3.png "Sysmon View")

The events can be even arranged (grouped) by machine, event type or GUID

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/4.png "Sysmon View")

The tool can support multiple grouping levels

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/5.png "Sysmon View")


**Getting Started**

To get started, export Sysmon events to XML file using the built-in WEVTUtil, this file will be imported later by Sysmon View:

`WEVTUtil query-events “Microsoft-Windows-Sysmon/Operational” /format:xml /e:sysmonview > eventlog.xml`

Once exported, run Sysmon View and import the generated file “eventlog.xml” (or the name you selected), please note that this might take some time, depending on the size of the log file (the file needs to be imported once, subsequent runs of Sysmon View do not require importing the data again, just use the file menu `File -> Load existing data` to load previously imported data again).

All data will be imported to a _SQLite_ database file named **SysmonViewDB** that resides in the same location as Sysmon View executable. This file can be shared with others if required, just place the file in the same location as  Sysmon View and use the command `File -> Load existing data`.

Each time a new XML file is imported, the database file will be deleted and re-created. To preserve any previously imported data, copy the database file to another location or simply rename it.

The database can be used directly in your own applications too, the database contains summaries of hashes, executables, IP addresses, geo mappings and all are logically linked through a file name or a session (executable GUID).

**Hint**: _You can query the database file directly using any SQLite management software without the need for Sysmon View, for example, to generate reports or analyze data_

**Experimental - Sysmon View and Elasticsearch**

Sysmon View version 1.5 can import Sysmon events from Elasticsearch using Winlogbeat. To get started, configure Winlogbeat to log Sysmon events to an Elasticsearch instance and create an index for "winlogbeats-*", then use the new Elasticsearch import from "File" menu. Good reference to this setup can be found [here](https://cyberwardog.blogspot.ae/2017/02/setting-up-pentesting-i-mean-threat_87.html).

This feature is currently in testing for several reasons:

*  The previous setup might be different than what is being adopted by others
*  Importing logs from Elasticsearch might impact the performance of the logs "visualization", this is still being tested
*  Connectivity to Elasticsearch needs to be "securely" improved(for example add support to SSL, X-Pack, etc...)

# Sysmon Shell

Sysmon Shell can aid in writing and applying Sysmon XML configuration through a simple GUI interface.

![Sysmon Shell](https://nosecurecode.blog/wp-content/uploads/2017/11/HeadImageSysmonShell.png "Sysmon Shell")

Sysmon Shell can also be used to explore more about Sysmon configuration options available with each release, easily apply and update the configuration, and export Sysmon logs, in a nutshell:

* Sysmon Shell can load Sysmon XML files configurations: current version supports all Sysmon schemas. In addition (the tool won’t load any configuration directly from registry - might add support to this feature in the future).
* It can export/save the final XML to a file.
* It can apply the generated XML file by calling Sysmon.exe -c command directly (creating a temporary XML file in the same folder where Sysmon is installed), for this reason, it will need elevated privileges (the need for this is inherited from Sysmon process itself), the output of applying the configuration will be displayed in the preview pan (this is Sysmon output)
* XML Configuration can be previewed before saving in the preview pane
* In case Sysmon is being used to do malware analysis, the last tab (marked "Logs Export") might be found handy, as it allows exporting Sysmon event logs to XML, which can be later used with "Sysmon View" for events analysis, the export has three options:
    * Export only
    * Export and clear Sysmon event log
    * Export, backup evtx file and clear the event log
* The utility has descriptions for all events types taken from Sysmon Sysinternals home page (https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
* Sysmon Shell comes bundled with many Sysmon configuration templates created by other security professionals

![Sysmon Shell Templates](https://nosecurecode.blog/wp-content/uploads/2017/12/SysmonShellTemplates.png "Sysmon Shell Templates")

**What it won’t do**: warn you about Include/Exclude conflicts or attempt to validate the rules itself, however, once the configuration is applied, the preview pane will show the output captured from Sysmon command execution (this is the output of `Sysmon -c command`), from which errors can be identified

# Notes
* Password of archives is **password**
* All executables are **digitally signed**
* All executables are **packed to reduce their size** (all dependencies are statically linked for portability)

# Additional Resources
* You can read more about Sysmon View & Sysmon Shell on my blog https://nosecurecode.com/
* The following is a good overview about Sysmon and Sysmon View https://www.fwhibbit.es/sysmon-the-big-brother-of-windows-and-the-super-sysmonview

# Support
Sysmon Tools are free, I try my best to maintain the code base, track changes in Sysmon, resolve bugs as soon as they are reported, and reply to all queries