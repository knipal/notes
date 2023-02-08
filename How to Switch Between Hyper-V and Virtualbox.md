# How to Switch Between Hyper-V and Virtualbox

To disable Hyper-V and enable Virtualbox
```
bcdedit /set hypervisorlaunchtype off
```

To disable Virtualbox and enable Hyper-V
```
bcdedit /set hypervisorlaunchtype auto start
```