# 

```` ps1
function Expand-Zip ($File)
{
	$shell = New-Object -ComObject Shell.Application
	# must be the full filename for this to work
	$zipFile = $shell.NameSpace($((Get-Item $File).FullName))
	foreach($item in $zipFile.Items())
	{
		$shell.Namespace($((get-item .).FullName)).CopyHere($item)
	}
}

if (-Not $(Test-Path ".\2005")) { mkdir ".\2005" }
if (-Not $(Test-Path ".\2005\x86")) { mkdir ".\2005\x86" }
if (-Not $(Test-Path ".\2005\x64")) { mkdir ".\2005\x64" }
if (-Not $(Test-Path ".\2008")) { mkdir ".\2008" }
if (-Not $(Test-Path ".\2010")) { mkdir ".\2010" }
if (-Not $(Test-Path ".\2012")) { mkdir ".\2012" }
if (-Not $(Test-Path ".\2013")) { mkdir ".\2013" }

Invoke-WebRequest -Uri http://downloads.sourceforge.net/sevenzip/7za920.zip -OutFile 7za920.zip -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Expand-Zip -File 7za920.zip

Invoke-WebRequest -Uri http://download.microsoft.com/download/8/B/4/8B42259F-5D70-43F4-AC2E-4B208FD8D66A/vcredist_x86.EXE -OutFile .\2005\x86\vcredist_x86.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/8/B/4/8B42259F-5D70-43F4-AC2E-4B208FD8D66A/vcredist_x64.EXE -OutFile .\2005\x64\vcredist_x64.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/5/D/8/5D8C65CB-C849-4025-8E95-C3966CAFD8AE/vcredist_x86.exe -OutFile .\2008\vcredist_x86.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/5/D/8/5D8C65CB-C849-4025-8E95-C3966CAFD8AE/vcredist_x64.exe -OutFile .\2008\vcredist_x64.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/1/6/5/165255E7-1014-4D0A-B094-B6A430A6BFFC/vcredist_x86.exe -OutFile .\2010\vcredist_x86.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/1/6/5/165255E7-1014-4D0A-B094-B6A430A6BFFC/vcredist_x64.exe -OutFile .\2010\vcredist_x64.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe -OutFile .\2012\vcredist_x86.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe -OutFile .\2012\vcredist_x64.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x86.exe -OutFile .\2013\vcredist_x86.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox
Invoke-WebRequest -Uri http://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe -OutFile .\2013\vcredist_x64.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::FireFox

Start-Process -FilePath .\7za.exe -ArgumentList "x -r .\2005\x86\vcredist_x86.exe -o"".\2005\x86""" -NoNewWindow -Wait
Start-Process -FilePath .\7za.exe -ArgumentList "x -r .\2005\x64\vcredist_x64.exe -o"".\2005\x64""" -NoNewWindow -Wait
````

````
<?xml version="1.0" encoding="UTF-8"?>
<Wix xmlns="http://schemas.microsoft.com/wix/2006/wi" xmlns:util="http://schemas.microsoft.com/wix/UtilExtension">
    <Bundle Name="Microsoft Visual C++ Redistributables Package" Version="1.0.0.0" Manufacturer="Tim de Vries" UpgradeCode="cf68097f-ba14-4939-8f00-7bc90367d40d" DisableModify="yes"
			Condition="VersionNT &gt;= &quot;6.0&quot;" HelpUrl="http://support.microsoft.com/kb/2019667" Copyright="Copyright © 2014, Tim de Vries (package only)" AboutUrl="http://support.microsoft.com/kb/2019667">
        <BootstrapperApplicationRef Id="WixStandardBootstrapperApplication.HyperlinkLicense" />
        <WixVariable Id="WixStdbaLicenseUrl" Value="" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{5FCE6D76-F5DC-37AB-B2B8-22AB8CEDB1D4}" Value="DisplayVersion" Variable="VCRedist2008x64" Win64="yes" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{9BE518E6-ECC6-35A9-88E4-87755C07200F}" Value="DisplayVersion" Variable="VCRedist2008x86" Win64="no" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{1D8E6291-B0D5-35EC-8441-6616F567A0F7}" Value="DisplayVersion" Variable="VCRedist2010x64" Win64="yes" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{F0C3E5D1-1ADE-321E-8167-68EF0DE699A5}" Value="DisplayVersion" Variable="VCRedist2010x86" Win64="no" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{37B8F9C7-03FB-3253-8781-2517C99D7C00}" Value="DisplayVersion" Variable="VCRedist2012x64" Win64="yes" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{B175520C-86A2-35A7-8619-86DC379688B9}" Value="DisplayVersion" Variable="VCRedist2012x86" Win64="no" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{929FBD26-9020-399B-9A7A-751D61F0B942}" Value="DisplayVersion" Variable="VCRedist2013x64" Win64="yes" />
        <util:RegistrySearch Root="HKLM" Key="SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{F8CFEB22-A2E7-3971-9EDA-4B11EDEFC185}" Value="DisplayVersion" Variable="VCRedist2013x86" Win64="no" />
        <Chain>
			<!-- Visual C++ Redistributable 2005 doesn't support uninstall in EXE format, so extract the files and install the MSI -->
            <MsiPackage Id="msi9f83d9f2626b4fccbbb22cf1d3f848c0" SourceFile="2005\x86\vcredist.msi" DisplayName="Visual C++ Runtime 8.0.50727.6195 x86" Visible="yes" />
            <MsiPackage Id="msi7b6bdd30b3ef47eab49a5da3b878f029" SourceFile="2005\x64\vcredist.msi" DisplayName="Visual C++ Runtime 8.0.50727.6195 x64" Visible="yes" />
            <!-- The remaing Visual C++ Redistributables don't support directly installing using the MSI, so we invoke the EXE file -->
            <ExePackage Id="exe7e264baeaad5442b80bc3d5d2d90d31a" DetectCondition="VCRedist2008x86 = &quot;9.0.30729.6161&quot;" SourceFile="2008\vcredist_x86.exe" DisplayName="Visual C++ Redistributable 9.0.30729.6161 x86" InstallCommand="/q" UninstallCommand="/qu" />
            <ExePackage Id="exe522ac6f820c54300be35e2ae562e899c" DetectCondition="VCRedist2008x64 = &quot;9.0.30729.6161&quot;" SourceFile="2008\vcredist_x64.exe" DisplayName="Visual C++ Redistributable 9.0.30729.6161 x64" InstallCommand="/q" UninstallCommand="/qu" />
            <ExePackage Id="exe60c0db1b0bfd4376a4471544b71a4369" DetectCondition="VCRedist2010x86 = &quot;10.0.40219&quot;" SourceFile="2010\vcredist_x86.exe" DisplayName="Visual C++ Redistributable 10.0.40219 x86" InstallCommand="/q /norestart" UninstallCommand="/q /uninstall /norestart" />
            <ExePackage Id="exe53b0790c155c45d28464d6ddaa9dc595" DetectCondition="VCRedist2010x64 = &quot;10.0.40219&quot;" SourceFile="2010\vcredist_x64.exe" DisplayName="Visual C++ Redistributable 10.0.40219 x64" InstallCommand="/q /norestart" UninstallCommand="/q /uninstall /norestart" />
            <ExePackage Id="exe3e6c677a7591496eb02b00c8631718db" DetectCondition="VCRedist2012x86 = &quot;11.0.61030&quot;" SourceFile="2012\vcredist_x86.exe" DisplayName="Visual C++ Redistributable 11.0.61030 x86" InstallCommand="/install /quiet /norestart" UninstallCommand="/quiet /uninstall /norestart" />
            <ExePackage Id="exe95ba2c7fbd884254a2378a435ecdcaec" DetectCondition="VCRedist2012x64 = &quot;11.0.61030&quot;" SourceFile="2012\vcredist_x64.exe" DisplayName="Visual C++ Redistributable 11.0.61030 x64" InstallCommand="/install /quiet /norestart" UninstallCommand="/quiet /uninstall /norestart" />
            <ExePackage Id="exe8873fd00fd9f4111908474d8d27102d2" DetectCondition="VCRedist2013x86 = &quot;12.0.21005&quot;" SourceFile="2013\vcredist_x86.exe" DisplayName="Visual C++ Redistributable 12.0.21005 x86" InstallCommand="/install /quiet /norestart" UninstallCommand="/quiet /uninstall /norestart" />
            <ExePackage Id="exe53f2a9a789f34bdba696ccda4c5da315" DetectCondition="VCRedist2013x64 = &quot;12.0.21005&quot;" SourceFile="2013\vcredist_x64.exe" DisplayName="Visual C++ Redistributable 12.0.21005 x64" InstallCommand="/install /quiet /norestart" UninstallCommand="/quiet /uninstall /norestart" />            
        </Chain>
    </Bundle>
</Wix>
````

```` ps1
candle.exe -ext WixBalExtension -ext WixUtilExtension "MicrosoftVCRedist_1.0.0.0.wix"
light.exe -ext WixBalExtension -ext WixUtilExtension -o "MicrosoftVCRedist_1.0.0.0.exe" "MicrosoftVCRedist_1.0.0.0.wixobj" 
````
