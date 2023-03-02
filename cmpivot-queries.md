Resources to check first
- https://systemcenterdudes.com/sccm-cmpivot-query/


Find duplicate SusClientID (WSUS guid)
```
Registry('HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate')
| where Property == ('SusClientID')
| project Device,SusClientID=Value
| summarize dcount(Device) by SusClientID
| where dcount_ > 1
| order by dcount_
```

Find .NET 4 Version ([Reference](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed#minimum-version))
```
Registry('HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full')
| where Property == ('Release')
| project Device,Release=Value
```

Check the status of a service (make sure it's running)
```
Service
| where Name == 'ccmexec'
| where State != 'Running'
```

Check that all automatic services are running
```
Service
| where StartMode == 'Auto'
| where State != 'Running'
```

Get Timezone
```
ComputerSystem
| project Device,Domain,CurrentTimeZone
```

Get Non-AD local Adminstrators
```
Administrators
| where (ObjectClass == 'User')
| where (PrincipalSource == 'ActiveDirectory')
```

Get UEFI Boot Machines
```
Partition
| where BootPartition == true
| where DiskIndex == 0
| where Type == 'GPT: System'
| project SystemName,BootPartition,DiskIndex,Type
```

Get Legacy BIOS Boot Machines
```
Partition
| where BootPartition == true
| where DiskIndex == 0
| where Type == 'Installable File System'
| project SystemName,BootPartition,DiskIndex,Type
```

Get DNS Servers (Client DNS Setting)
```
IPConfig
| project Device,InterfaceAlias,IPV4Address,DNSServerList
```



Find Expired Certificates
```
EventLog('Application')
| where EventID==64
| distinct Device
```