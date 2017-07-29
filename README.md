# Sysmon View

Sysmon View helps in tracking and visualizing Sysmon logs

**Getting Started**

To get started, we need to export Sysmon events to XML file using WEVTUtil, this file will be imported later by Sysmon View:

`WEVTUtil query-events “Microsoft-Windows-Sysmon/Operational” /format:xml /e:sysmonview > eventlog.xml`

Once exported, run Sysmon View and import the generated file “eventlog.xml” (or the name you selected), please note that this might take some time, depending on the size of the log file (this needs to be done once per log file, subsequent runs of Sysmon View do not require importing the data again, just use the command `File -> Load existing data` to load previous data and work with it again)

All data will be exported to a database file _(SQLite)_ named **SysmonViewDB** that resides in the same location as Sysmon View. This file can be shared with others, just place the file in the same location as  Sysmon View and use the command `File -> Load existing data`

Password of archives is **password**

You can read more about Sysmon View and it's use cases on my website https://nosecurecode.com/
