---
layout: post
title: Unattended Installation Tips
---

## EXE Installers
### Generic
Check the vendors website for any installation guides, or try the following switches on the command line to see if help is available:

~~~~
/help
/h
-help
--help
-h
-?
--?
/?
~~~~
 
If a silent installation command cannot be determined, quite often one of the ones listed below may work:

~~~~
/S
/s
/Q
/q
/silent
-silent
--silent
-quiet
--quiet
/quiet
/q
/passive
~~~~

### Installshield Based Installers
#### Basic MSI

~~~~ bat
:: creates a setup file in %WINDIR%
installer.exe /r

:: install silently 
installer.exe /s /f1<path to response file>
~~~~

#### InstallScript MSI 
~~~~ bat
:: you can use MSI properties after the /v to pass them to the installer
installer.exe /s /v"/qb-"

installer.exe /s /v"REBOOT=ReallySuppress TRANSFORMS=sample.mst /qn"
~~~~

### Innosetup
~~~~ bat
:: accept defaults and show progress of installation
installer.exe /silent /norestart

:: accept defaults and don't show progress of installation
installer.exe /verysilent /norestart
~~~~

### NSIS
~~~~ bat
:: entirely depends on the package author whether this works
installer.exe /S
~~~~

## MSU
TODO

## Windows Installer
Install the [PsMSI cmdlets](https://psmsi.codeplex.com/) for PowerShell.

### Install an MSI package
~~~~ bat
msiexec /i "Orca.msi"
~~~~

To have it run automatically accepting the default settings:

* append `/qn` to install completely silent, or
* append `/qb-` to display a small progress window with a cancel button, or
* append `/qb!` to display a small progress window with no cancel button.

~~~~ bat
msiexec /i "Orca.msi" REBOOT=ReallySuppress /qn
~~~~

#### Customizing the Installation from the Install String
Append properties to the install command to manipulate how the MSI installs. Properties can be found using the following command:

~~~~ ps1
# requires PsMSI cmdlets
Get-MSITable -Table Property -Path ".\Oracle Virtualbox Installer.msi" | sort Property
~~~~

Append the property to the installation command line to perform the manipulation:

~~~~ bat
msiexec /i "Orca.msi" REBOOT=ReallySuppress DESKTOPSHORTCUT=0 /qn
~~~~

Multiple properties can be defined like this, although there is a character limit to the length of the install command.

#### Customizing the Installation with an MST
TODO

### Upgrade an MSI package
|| Type of update || Product Code || Product Version
|-|-|-
| Small Update | No change | No change
| Minor Upgrade | No change | Changed
| Major Upgrades | Changed | Changed

### Small and Minor Upgrades
When a Windows Installer msi or patch is referred to as a small or minor upgrade, attempting to install the upgrade will results in a "product is already installed" message. The following command line is an example of how to apply the updated msi or patch without removing the software first: 

~~~~ bat
msiexec /i SampleUpgrade2.msi REINSTALL=ALL REINSTALLMODE=vomus REBOOT=ReallySuppress
~~~~

### Major Upgrades
A major upgrade removes the earlier version of the software first.

### Removing an MSI package
An MSI package can be removed by referencing the original `MSI` installation file, or if this is unavailable, use the product code

~~~~ ps1
# requires PsMSI cmdlets
Get-MSIProductInfo -Name *citrix*

ProductCode                            ProductVersion      ProductName
-----------                            --------------      -----------
{196AD722-9731-48D9-B50E-6B249AD0B391} 14.1.0.0            Citrix Receiver(SSON)
{C4E28723-0663-4012-9BDC-E21A14C1316C} 14.1.0.0            Citrix Receiver (HDX Flash Redirection)
{0E1C5B43-1837-4F98-A96B-79A8A0A5955F} 14.1.0.0            Citrix Receiver(USB)
{D9EE360A-7C19-47EC-93C7-97DEFF64804B} 4.1.0.56471         Citrix Receiver Inside
{ADE8A83D-BB70-4FB5-BA19-26C47EA31894} 14.1.0.0            Citrix Receiver(DV)
{CA55005D-94AC-4596-9646-679D6CC0D620} 5.1.0.62606         Citrix Authentication Manager
{012C59CF-074A-43DA-8085-B6E636733B59} 14.1.0.0            Citrix Receiver(Aero)
~~~~

To remove this package, the following command may be used to remove it silently:

~~~~ bat
msiexec /x {012C59CF-074A-43DA-8085-B6E636733B59} REBOOT=ReallySuppress /qn
~~~~

## App-V 5.0
### Install an App-V package for all users
~~~~ ps1
# This example assumes you have navigated to the folder in which the App-V package resides
Add-AppvClientPackage -Path .\dotPDN_Paint.NET_3.5.11_1.0_V.appv -DynamicDeploymentConfiguration `
.\dotPDN_Paint.NET_3.5.11_1.0_V_DeploymentConfig.xml | Publish-AppvClientPackage -Global
~~~~

### Removing an App-V package
~~~~ ps1
# In this example, we search for a package with a name like *dotpdn* and remove it
# If the search term matched multiple packages, they would all be removed
Get-AppvClientPackage -Name *dotpdn* -All | Remove-AppvClientPackage
~~~~

~~~~ ps1
# As above, but if the package is in use, unpublish it first and reboot or logoff. You can then remove the package
Get-AppvClientPackage -Name *dotpdn* | Unpublish-AppvClientPackage -Global
~~~~
