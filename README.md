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

-   There are 32 and 64-bit builds (32.zip and 64.zip)
-   All executables are **digitally signed**
-   Sysmon Tools **ARE FREE**

# Sysmon View

Sysmon View helps in tracking and visualizing Sysmon logs by logically grouping and linking the various events generated using executable names, session GUIDs or the time of the event, it also has easy to use search feature to look through all the events data, a GEO mapping of IP addresses and VirusTotal lookup for IP, domain and hashes.

![Sysmon View](https://nosecurecode.blog/wp-content/uploads/2017/11/HeadImage.png "Sysmon View")

**Getting Started**

To get started, export Sysmon events to XML file using WEVTUtil, this file will be imported later by Sysmon View:

`WEVTUtil query-events “Microsoft-Windows-Sysmon/Operational” /format:xml /e:sysmonview > eventlog.xml`

Once exported, run Sysmon View and import the generated file “eventlog.xml” (or the name you selected), please note that this might take some time, depending on the size of the log file (the file needs to be imported once per log file, subsequent runs of Sysmon View do not require importing the data again, just use the command `File -> Load existing data` to load previous data and work with it again).

All data will be exported to a database file _(SQLite)_ named **SysmonViewDB** that resides in the same location as Sysmon View executable. This file can be shared with others if required, just place the file in the same location as  Sysmon View and use the command `File -> Load existing data`.

Each time a new XML file is imported, the database file will be deleted and re-created, to preserve any previously imported data, copy the database file to another location or simply rename it.

The database can be used directly in your own applications too, the database contains summaries of hashes, executables, IP addresses, geo mappings and all are logically linked through a file name or a session (executable GUID).

**Experimental - Sysmon View and Elasticsearch**

Sysmon View version 1.5 can now connect to Elasticsearch and import Sysmon events. To get started, configure Winlogbeat to log Sysmon events to an Elasticsearch instance and create an index for "winlogbeats-*", then use the new Elasticsearch import from "File" menu. Good reference to this setup can be found [here](https://cyberwardog.blogspot.ae/2017/02/setting-up-pentesting-i-mean-threat_87.html).
It's worth mentioning another update, since the tool can now import directly from Elasticsearch, multiple instances of Sysmon might be deployed on several endpoints, so the events are now being visualized per endpoint.

This feature is currently in testing for several reasons:

*  The setup might be different than previously mentioned, some configured logging utilities other than Winlogbeats, some setup is logging to Logstach instead of logging directly to Elasticsearch, etc...
*  Importing logs from Elasticsearch might impact the performance of the logs "visualization", which might need enhancements if proven true
*  Connectivity to Elasticsearch needs to be improved to add more security (for example SSL, X-Pack, etc...)

# Sysmon Shell

Sysmon Shell can aid in writing and applying Sysmon XML configuration through a simple GUI interface.

![Sysmon Shell](https://nosecurecode.blog/wp-content/uploads/2017/11/HeadImageSysmonShell.png "Sysmon Shell")

Sysmon Shell can also be used to learn more about Sysmon configuration options available with each release, easily apply and update the configuration, and export Sysmon logs, in a nutshell:

* Sysmon Shell can load Sysmon XML files configurations: current version supports all Sysmon schemas. In addition (the tool won’t load any configuration directly from registry - might add support to this feature in the future).
* It can export/save the final XML to a file.
* It can apply the generated XML file by calling Sysmon.exe -c command directly (creating a temporary XML file in the same folder where Sysmon is installed), for this reason, it will need elevated privileges (the need for this is inherited from Sysmon process itself), the output of applying the configuration will be displayed in the preview pan (this is Sysmon output)
* XML Configuration can be previewed before saving in the preview pane
* In case Sysmon is being used to do malware analysis, the last tab (marked "Logs Export") might be found handy, as it allows exporting Sysmon event logs to XML, which can be later used with "Sysmon View" for events analysis, the export has three options:
    * Export only
    * Export and clear Sysmon event log
    * Export, backup evtx file and clear the event log
* The utility has descriptions for all events types taken from Sysmon Sysinternals home page (https://technet.microsoft.com/en-us/sysinternals/sysmon)
* Sysmon Shell comes bundled with many Sysmon configuration templates created by other security professionals

![Sysmon Shell Templates](https://nosecurecode.blog/wp-content/uploads/2017/12/SysmonShellTemplates.png "Sysmon Shell Templates")

**What it won’t do**: warn you about Include/Exclude conflicts or attempt to validate the rules itself, however, once configuration is applied, the preview pane will show the output captured from Sysmon command execution (this is the output of `Sysmon -c command`), from which errors can be identified

# Additional Resources

* You can read more about Sysmon View & Sysmon Shell on my website https://nosecurecode.com/
* For an overview on Sysmon and Sysmon View in action, check out https://www.fwhibbit.es/sysmon-the-big-brother-of-windows-and-the-super-sysmonview

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