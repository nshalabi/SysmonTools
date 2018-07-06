# Sysmon View

Sysmon View helps in tracking and visualizing Sysmon logs by logically grouping and correlating the various Sysmon events together, using existing events data, such as executables names, session GUIDs, event creation time, etc., the tool then re-arranges this data for display into multiple views

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/1.png "Sysmon View")

**Getting Started**

To get started, export Sysmon events to XML file using the built-in WEVTUtil, this file will be imported later by Sysmon View:

`WEVTUtil query-events “Microsoft-Windows-Sysmon/Operational” /format:xml /e:sysmonview > eventlog.xml`

Once exported, run Sysmon View and import the generated file “eventlog.xml” (or the name you selected), please note that this might take some time, depending on the size of the log file (the file needs to be imported once, subsequent runs of Sysmon View do not require importing the data again, just use the file menu `File -> Load existing data` to load previously imported data again).

All data will be imported to a _SQLite_ database file named **SysmonViewDB** that resides in the same location as Sysmon View executable. This file can be shared with others if required, just place the file in the same location as  Sysmon View and use the command `File -> Load existing data`.

Each time a new XML file is imported, the database file will be deleted and re-created. To preserve any previously imported data, copy the database file to another location or simply rename it.

The database can be used directly in your own applications too, the database contains summaries of hashes, executables, IP addresses, geo mappings and all are logically linked through a file name or a session (executable GUID).

**Hint**: _You can query the database file directly using any SQLite management software without the need for Sysmon View, for example, to generate reports or analyze data_

**Experimental - Sysmon View and Elasticsearch**

Sysmon View version 1.5 can import Sysmon events from Elasticsearch. To get started, configure Winlogbeat to log Sysmon events to an Elasticsearch instance and create an index for "winlogbeats-*", then use the new Elasticsearch import command from the "File" menu. Good reference to this setup can be found [here](https://cyberwardog.blogspot.ae/2017/02/setting-up-pentesting-i-mean-threat_87.html).

This feature is currently in testing for several reasons:

*  The previous setup might be different than what is being adopted by others
*  Importing logs from Elasticsearch might impact the performance of the logs "visualization", this is still being tested
*  Connectivity to Elasticsearch needs to be "securely" improved (for example add support to SSL, X-Pack, etc...)

**Sysmon Views**

**Process View** this view simply helps focus on a summary of "run sessions", for example, the analyst can start with executable name (such as cmd.exe) or event type (such as Network event), from there, further filtering can be applied, for example, finding running sessions originating for the same binary, but from different locations. This view utilizes the process GUID to filter events per session "run", selecting any running session (from the list of GUIDs) will show all other related (correlated) events in a simple data-flow-like view, sorted using the time of the event.
_Note: in case data is being imported from an Elasticsearch instance instead of single machine, events can be arranged per executable per machine - check previous section **"Experimental - Sysmon View and Elasticsearch"**)._

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/1_5.png "Sysmon View")

Access to Sysmon event details is provided by simply double-clicking any event in the view, for example, the previous screen capture shows the details of the _Process Creation_ event (event ID 1), the tool also can integrate with VirusTotal upon demand for further hash and IP lookup (Needs an API key registration).

**Map View** : During the events import process, there is an option to geo-locate IP addresses, if set, Sysmon View will try to geo-map **Network Destinations** using https://freegeoip.net service.

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/2.png "Sysmon View")

In **map view**, it is easy to navigate between correlated (related) events by using a _network event_ as a starting point, again, the tool is able to achive this using the running process session GUID. To explore related events, use the hyperlinks for the session GUID, a new view similar to **process view** will show up in a new window with all related session events:

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/2_5.png "Sysmon View")

**All Events View** can also be used to do a _full search_ through all Sysmon collected events data, it also help in viewing events that do not relate to other events, such as the "Driver Loaded" event type. Navigation between related events is still provided using the process GUID in addition to event details by clicking on _FID link_

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/3.png "Sysmon View")

Additionally, The **All Events View** supports pivot-like (grouping) arrangement of events, by machine name, event type or GUID, as shown below

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/4.png "Sysmon View")

Multiple grouping levels are also possible

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2018/07/5.png "Sysmon View")

# Sysmon Shell

Sysmon Shell can aid in writing and applying Sysmon XML configurations through a simple GUI interface.

![Sysmon Shell](https://nosecurecode.blog/wp-content/uploads/2017/11/HeadImageSysmonShell.png "Sysmon Shell")

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

![Sysmon Shell Templates](https://nosecurecode.blog/wp-content/uploads/2017/12/SysmonShellTemplates.png "Sysmon Shell Templates")

**What it won’t do**: warn you about Include/Exclude conflicts or attempt to validate the rules itself, however, once the configuration is applied, the preview pane will display the output captured from Sysmon.exe when configuration is applied (the output of `Sysmon -c command`), from which errors can be identified

# Notes
* Password of archives is **password**
* All executables are **digitally signed**
* All executables are **packed to reduce their size** as all dependencies are statically linked for portability

# Additional Resources
* You can read more about Sysmon View & Sysmon Shell on my blog https://nosecurecode.com/
* The following is a good overview about Sysmon and Sysmon View https://www.fwhibbit.es/sysmon-the-big-brother-of-windows-and-the-super-sysmonview

# Support
Sysmon Tools are free, I try my best to maintain the code base, track changes in Sysmon, resolve bugs as soon as they are reported, and reply to all queries