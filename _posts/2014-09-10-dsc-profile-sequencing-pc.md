---
layout: post
title: DSC script for App-V 5.0 Sequencing PC
---

## Script
~~~~ ps1
$config = @{
	AllNodes = @(
		@{
			NodeName = "localhost";
			PSDscAllowPlainTextPassword = $true
		}
)}

Configuration SequencerTemplate
{
	param ($ComputerName)

	# remember to update the default password
	$password = ConvertTo-SecureString "passwordinplaintext" -AsPlainText -Force
	$cred = New-Object System.Management.Automation.PSCredential ("packager", $password)
	
    Node $ComputerName
    {
        Service WindowsUpdateConfig
        {
            Name = "wuauserv"
            StartupType = "Disabled"
            State = "Stopped"
        }

        Service WindowsSearchConfig
        {
            Name = "WSearch"
            StartupType = "Disabled"
            State = "Stopped"
        }

        Service SecurityCentreConfig
        {
            Name = "wscsvc"
            StartupType = "Disabled"
            State = "Stopped"
        }
        
		Registry PowerShellConfig1
		{
			Ensure = "Present"
			Key = "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\PowerShell"
			ValueName = "EnableScripts"
			ValueData = "1"
			ValueType = "Dword"
		}

		Registry PowerShellConfig2
		{
			Ensure = "Present"
			Key = "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\PowerShell"
			ValueName = "ExecutionPolicy"
			ValueData = "Bypass"
			ValueType = "String"
		}

		Registry WindowsDefenderDisable
		{
			Ensure = "Present"
			Key = "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender"
			ValueName = "DisableAntiSpyware"
			ValueData = "1"
			ValueType = "Dword"
		}
		
        User PackageUser
        {
            UserName = "packager"
            Ensure = "Present"
            Password = $cred
            PasswordChangeNotAllowed = $true
            PasswordChangeRequired = $false
            PasswordNeverExpires = $true
        }

        Group AdminGroupMembership
        {
            GroupName = "Administrators"
            Ensure = "Present"
            MembersToInclude = "packager"
            DependsOn = "[User]PackageUser"
        }

        Package Orca
        {
            Name = "Orca"
            Path = "C:\dsc\orca.msi"
            ProductId = "85F4CBCB-9BBC-4B50-A7D8-E1106771498D"
            Ensure = "Present"
        }

        Package Notepad2-mod
        {
            Name = "Notepad2-mod 4.2.25.906"
            Path = "C:\dsc\Notepad2-mod.4.2.25.906.exe"
            ProductId = ""
            Arguments = "/verysilent"
            Ensure = "Present"
        }

        Package AppvSequencer
        {
            Name = "Microsoft Application Virtualization (App-V) Sequencer 5.0 Service Pack 2"
            Path = "C:\dsc\AppV5.0SP2-Sequencer-KB2956985.exe"
            ProductId = ""
            Arguments = "/ACCEPTEULA=1 /CEIPOPTIN=0 /q"
            Ensure = "Present"
        }
    }
}
~~~

## Usage
`SequencerTemplate -ConfigurationData $config -ComputerName localhost`
