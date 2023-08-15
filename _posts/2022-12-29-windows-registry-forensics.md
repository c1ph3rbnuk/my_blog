---
layout: article
title: Windows Registry Forensics Cheatsheet
article_type: BlogPosting
date: 2022-12-29
cover: "/assets/images/winreg.png"
permalink: /:title.html
tags: "forensics windows registry"
author: c1ph3rbnuk
---

### The Registry -
The windows registry located at **`Windows\system32\config`** is a large database that stores configuration settings on Microsoft Windows Operating system and its applications. It is used to store a variety of information such as system settings, user preferences, etc...In a live system, the registry can be accessed using `regedit`(Registry Editor, a built-in tool in windows operating system). RegRipper can be used to browse the registry of an image file.


### Structure   
It is organized as a tree structure with keys and subkeys that represent the hierachy. The root of the registry is called `HKEY` and it has several subkeys that represent diferent areas of the system.
1. Hives - a hive is a logical group of keys, subkeys and values in the registry. The following are the predefined hives in the windows registry: 
    - HKEY_LOCAL_MACHINE(HKLM) : stores configuration information that is specific to the local computer.
    - HKEY_CURRENT_USER(HKCU) : stores configuration information that is specific to the current user.
    - HKEY_CLASSES_ROOT(HKCR) : stores file associations and OLE Object class definations.
    - HKEY_USERS(HKU) : stores configuration information for all users of the computer.  


2. Keys - a key is a container that stores specific configuration information and values. eg `HKEY_LOCAL_MACHINE\Software` this key contains information about installed software and their configuration settings.  


3. Subkeys - subkeys are located beneath another keys. They are children of keys.  


4. Values - a value is a name-value pair containing specific configuration information.  
  
  ---
    

**Registry Forensics** is the process of analyzing and interpreting data stored in windows registry inorder to gather evidence in a forensics investigation. The registry keys and values may contain a wealthy of information and artifacts left behind from the attacker interaction with the compromised machine that help proove the existence of an attack or malicious activities in a system. As a forensics investigator, sometimes you may need to focus on specific sections of this registry while conducting an investigation.  
  
  Below is a list of the common keys that may be on interest:-  
    
| Artifacts | Reg-Key | Description |
| :----- |:------: | ------:
| MRUs or (Most Recently Used) | `Computer\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`  | Stores a list of the programs that the user has run recently, in the order that they were run.|  
| Typed path Locations | `Computer\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths` | Stores a list of the paths that the user has typed into the address bar of the File Explorer application.| 
| Recent file history | `Computer\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSaveMRU` | Store a list of the files and directories that the user has opened or saved recently using common dialog boxes, in the order that they were accessed | 
| Program usage tracking | `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count` | Stores a count of the number of times that a specific program or feature has been used by the user). |
| Last Registry Key Viewed | `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Applets\Rgedit\LastKey` | Stores the path to the last registry key that was accessed using the Registry Editor (Regedit) tool|
| Advanced settings | `SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced` | Stores advanced configuration settings for the File Explorer application.|  
| Auto-Run Programs | `HKLM\Software\Microsoft\Windows\CurrentVersion\Runonce,HKLM\Software\Microsoft\Windows\CurrentVersion\Run,HKCU\Software\Microsoft\Windows\CurrentVersion\Runonce,HKCU\Software\Microsoft\Windows\CurrentVersion\Run` | Specifies the programs that should be launched automatically when a user logs into their system. The programs listed in this key are automatically run when the user logs into the system.|  
| ShellBags | `HKCU\SOFTWARE\Microsoft\Windows\Shell\BagMRU,HKCU\SOFTWARE\Microsoft\Windows\Shell\Bags` | Store the information like the icons customizations, the look and the feel of the folder, window position, size, sorting methods, etc.. Shellbags are extremely useful to find traces about folders that were on the system and were deleted since shellbags persist on the system even after deleting the folder. **Shellbags Explorer** is a tool that is used to examine the ShellBags data. |  
| Mounted device information | `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR,HKLM\SYSTEM\CurrentControlSet\Enum\USB,HKLM\SOFTWARE\Microsoft\Windows Portable Devices\Devices,HKLM\SYSTEM\MountedDevices,HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\EMDMgmt` (on if the device is not SSD) | Contains information about USB mass storage devices that are connected to the system |  
| Timezone Information | `SYSTEM\ControlSet001\Control\TimeZone\Information` | Contains information about the time zone settings on a system |  
| Computer Name | `SYSTEM\ControlSet001\Control\ComputerName\ComputerName` | Contains information about the computer name of a system. |  
| Windows information | `SOFTWARE\Microsoft\WindowsNT\CurrentVersion` | Contains information about the system. | 
| Installed application | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\` | Contains information about installed applications on a system. |  
| Services installed | `SYSTEM\ControlSet001\Services\` | Contains information about the services that are installed on a system.|  
| Firewall status | `HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy`| Contains a subkey for each profile (i.e. Domain, Private, and Public) that is used by the firewall, and each subkey contains data about the firewall rules and settings for that profile. **EnableFirewall =0** means the firewall is disabled **EnableFirewall=1** means the firewall is enabled |  
| Remote Desktop Information | `HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server` | Contains a number of subkeys that contain data about the Remote Desktop service. |  
| Shares | `HKLM\SYSTEM\ControlSet001\Services\LanmanServer\Shares` | Contains information about the shared resources on a system. |  
| Network Configurations | `HKLM\SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces` | Contains information about the network interfaces on a system.| 
| Network List | `HKLM\Software\Microsoft\Windows NT\CurrentVersion\NetworkList` | Contains information about the networks to which a system has connected.|  
| User Accounts | `HKLM\SAM\Domains\Account\Users\Names\WDAGUtilityAccount` | Contains information about all user accounts on a system. |  
| Last logged in User | `HKLM\Software\Micrososft\Windows\CurrentVersion\Authentication\LogonUI\LastLoggedOnUser` | Contains information about the last user to log on to a system. |  
| User Account Control(UAC) | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System` | User Account Control (UAC) is a security feature in the Windows operating system that is designed to prevent unauthorized changes to the system. UAC prompts the user for consent or credentials when certain actions are performed that require administrator privileges. Attackers may try to diasble it. EnableLUA=0 (UAC disabled) EnableLUA=1(UAC enabled) |  
| LNK files | `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` | LNK (short for "link") is the file extension for shortcut files in the Windows operating system. Shortcut files are used to create a link to another file or folder on the system, and can be used to quickly access frequently used files or programs. LNK files can be an important source of information, as they can provide insight into the programs and files that were used on a system. Even for deleted files, they can show proof that a file existed before. LNK files contains a lot of data useful for investigation MAC times, original path of the file, size, serial number of volume, network volume share, mac address of host computer, etc… **Tools: exifTool, lnk_parser(from SIFT workstation) lnk_extractor(from Autopsy)** |  
| | | Jump lists are a feature in the Windows operating system that allow users to quickly access recently used files or programs. Jump lists are displayed as a list of items that are associated with a particular program or task. They can provide insight into the programs and files that were used on a system. **Tools: jump_list_parser & jump_list_extractor**|  
|Prefetch & Superfetch | `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters`,`HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\Superfetch` | Prefetcher and Superfetch are features in the Windows operating system that are designed to improve the performance of the system by preloading data and optimizing the use of memory. They can provide insight into the programs and files that were used on a system. |  
| System Resource Utilization Monitor | SRUM databse is located at `C:\Windows\System32\sru\SRUDB.dat` | It keeps the names and paths of every application that executes on your system even the ones the attackers deleted. **Tools srum_dump & srum_extractor** |   

---
Registry forensics overally draws from a pool of valuable information to an investigator that helps understand actions and activities that took place on a system, how they were initiated, time and date stamps, and more.  