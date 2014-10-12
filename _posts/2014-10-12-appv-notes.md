---
layout: post
title: Notes from tinkering with App-V
---
## App-V 5
### Event Logs

~~~~ ps1
Get-WinEvent "Microsoft-AppV-Client/Virtual Applications" -MaxEvents 20 | select TimeCreated, Message
~~~~

### Open command prompt in sandbox

~~~~ bat
cmd /k /appvve:d90b63d3-9b85-41ee-8c92-682a519e4e38_4b6838d7-517f-421f-b18e-10eda64a8b53`
~~~~

### Links

* [About Scripts in App-V 5.0 | Confessions of a Guru](http://www.tmurgent.com/TMBlog/?p=1154)
* [Scripting and Embedded Scripting for AppV 5.0 (Dynamic Deployment and User Configuration Scripting) - The Microsoft App-V Team Blog - Site Home - TechNet Blogs](http://blogs.technet.com/b/appv/archive/2012/12/10/scripting-and-embedded-scripting-for-appv-5-0-dynamic-deployment-and-user-configuration-scripting.aspx)
* [http://blogs.technet.com/b/virtualworld/archive/2013/11/21/how-to-automate-the-creation-of-app-v-5-0-debug-command-prompts.aspx](http://blogs.technet.com/b/virtualworld/archive/2013/11/21/how-to-automate-the-creation-of-app-v-5-0-debug-command-prompts.aspx)
* [Architecture and Version Support with App-V 5.0](http://blogs.technet.com/b/virtualvibes/archive/2014/01/07/architecture-and-version-support-with-app-v-5-0.aspx)
* [Current list of App-V 5.0 file versions](http://support.microsoft.com/kb/2900621)
* [Your guide to App-V 5.0 application publishing and client interaction](http://blogs.technet.com/b/appv/archive/2014/01/20/your-guide-to-app-v-5-application-publishing-and-client-interaction.aspx)
* [How to troubleshoot App-V 5.0 DeploymentConfig & UserConfig script deployment failures using AppV Manage](http://blogs.technet.com/b/appv/archive/2014/01/30/how-to-troubleshoot-app-v-5-0-deploymentconfig-amp-userconfig-script-deployment-failures-using-appv-manage.aspx)
* [App-V 5 Tools](http://www.tmurgent.com/appv/index.php/resources/tools#catid89)
* [Global Registry State Changes in App-V 5.0](http://blogs.technet.com/b/virtualvibes/archive/2014/02/05/global-registry-state-changes-in-app-v-5-0.aspx)


## App-V 4
### Set Affinity

~~~~ xml
<?xml version="1.0" standalone="no"?>
<SOFTPKG GUID="3D60F9B5-E500-44EC-B5CC-940AB5086BED" NAME="Core Land" VERSION="1.0">
     <IMPLEMENTATION>
          <CODEBASE HREF="FILE://\OracleJavaRuntime_1.4.2_17_4.sft" GUID="00259D3B-130E-43DE-821A-69AC0BD24662" PARAMETERS="/c start /affinity 1 c:\progra~2\intern~1\iexplore.exe https://mw-bi.lb.bcc.qld.gov.au/forms/frmservlet?config=clnd" FILENAME="%CSIDL_SYSTEM%\cmd.exe" SYSGUARDFILE="Oracle Java Runtime 1.4.2_17\osguard.cp" SIZE="67043691"/>
          <VIRTUALENV TERMINATECHILDREN="FALSE">
               <POLICIES>
                    <LOCAL_INTERACTION_ALLOWED>FALSE</LOCAL_INTERACTION_ALLOWED>
               </POLICIES>
               <ENVLIST/>
          </VIRTUALENV>
          <WORKINGDIR>%CSIDL_SYSTEM%</WORKINGDIR>
          <VM VALUE="Win32">
               <SUBSYSTEM VALUE="console"/>
          </VM>
          <OS VALUE="Win2008R2TS64"/>
          <OS VALUE="Win764"/>
     </IMPLEMENTATION>
     <DEPENDENCY>
          <CLIENTVERSION VERSION="4.6.0.0"/>
     </DEPENDENCY>
     <PACKAGE NAME="Oracle Java Runtime 1.4.2_17"/>
     <ABSTRACT/>
     <MGMT_SHORTCUTLIST>
          <SHORTCUT LOCATION="%CSIDL_STARTMENU%\Online Systems" FILENAME="" OVERRIDDEN="TRUE" DISPLAY="Core Land" ICON="%SFT_MIME_SOURCE%/OracleJavaRuntime_1.4.2_17 Icons/Core Land for IE.ico"/>
     </MGMT_SHORTCUTLIST>
     <MGMT_FILEASSOCIATIONS>
          <PROGIDLIST/>
          <FILEEXTENSIONLIST/>
     </MGMT_FILEASSOCIATIONS>
</SOFTPKG>
~~~~

