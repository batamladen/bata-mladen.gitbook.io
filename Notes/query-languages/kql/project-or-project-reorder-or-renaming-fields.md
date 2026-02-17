# project | project-reorder | renaming fields

### project

The project command shows us just the fields we selected. Because sometimes whole tables can be overwhelming and we maybe need just a few fields from there.



```
VMComputer
| where OperatingSystemFamily == "Windows"
| project HostName, OperatingSystemFamily, Cpus
```



***

### project-reorder

This command is kinda a spin-off of the project command, it reorders the fields that we specify at the beginning of the table, but does not remove the other fields.



```
VMComputer
| where OperatingSystemFamily == "Windows"
| project-reorder HostName, OperatingSystemFamily, Cpus
```

As a result the HostName, OperatingSystemFamily, Cpus fields would be at the beginnig and than the rest of the fields from the table.



***

### renaming fields

Renaming the fields is easy with the project command.

Its just like specifying a variable.

Write the name you want before the field name and connect them with = sign.



```
VMComputer
| where OperatingSystemFamily == "Windows"
| project Host=HostName, OS=OperatingSystemFamily, Cpus
```



As a result, the HostName and OperatingSystemFamily fields would be renamed.
