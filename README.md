![](https://github.com/urpatton/OSD-Wizard-UDI-Replacement/blob/main/Screenshots/OSDWizardDefault.png?raw=true)

## TOPIC
```
	about_OSDWizard
```

## SHORT DESCRIPTION
```
	OSD Wizard for SCCM Task Sequences
```

## LONG DESCRIPTION
```
	OSD wizard replacement for the MDT UDI wizard.
```

## PARAMETERS
```
-UDIWizardXMLFileName
	Name of the UDI Wizard XML answer file
	XML configuration file must be in the script root folder
	Accepted values: "UDIWizard_Config.xml" (where UDIWizard_Config.xml is the name of your UDI wizard XML file)

-CustomWizardTitle
	Sets a custom title for the OSD Wizard
	Use "_SMSTSPackageName" to set the wizard name to the name of the OSD Task Sequence
	Use "_SMSTSOrgName" to set the wizard name to the SCCM Org name

-LockComputerName
	Alias: -DisableComputerName
	Locks the computername text box.
	Computer name field will still populate with OSDComputerName or _SMSTSMachineName unless -DisableComputerNameRequirement is used
	NOTE: Computer name field is required by default. If OSDComputerName is not present _SMSTSMachineName is used
	Use "-SerializeName" and/with "-NamePrefix" to ensure field is populated. Or you may also use -DisableComputerNameRequirement parameter
	Accepted values: "True"

-DisableComputerNameRequirement
	Disables the requirement to enter text into the ComputerName textbox
	NOTE: This will prevent the computer name text box from populating
	Accepted values: "True"

-SerializeName
	Will compress and truncate all but the last 15 characters of the serial number.
	When used with "-NamePrefix", will truncate serial number so name is 15 characters
	NOTE: NamePrefix must be shorter than serial
	Accepted values: "True"

-NamePrefix
	OPTIONAL. Sets the name prefix when using SerializeName. Can only be used with -SerializeName
	Example: NamePrefix = "DeptA-" then Computer name will be "DeptA-SerialNumber"
	NOTE: NamePrefix must be shorter than serial
	Accepted values: Any String

-DomainName
	Sets the "OSDDomainName" variable.
	If set the Domain Name text box will be locked and field populated
	Accepted values: Any String

-DisableDomainName
	Disables the domain name text box without setting -DomainName
	Accepted values: "True"
	
-DisableOUSelection
	Disables the OU selection combobox
	Accepted values: "True"

-DisableUserName
	Disable username text box.
	Leave enable to set "OSDJoinAccount" variable.
	NOTE: If 'DisablePassword" is set this will set "OSDBuildaccount" in place of "OSDJoinAccount". This is to allow a separate OSD join account
	Accepted values: "True"

-DisablePassword
	Disable password text box
	Will set "OSDBuildaccount" in place of "OSDJoinAccount". This is to allow a separate OSD join account
	Accepted values: "True"
	Leave enabled to set "OSDJoinPassword" variable

-DisableDiskSelection
	Disable the disk selection drop down
	Accepted values: "True"

-DisableBitlocker
	Disables and unchecks the BitLocker checkbox
	BitLocker is not checked by default.
	Leave enabled to set "OSDBitlockerMode" = "True"
	Accepted values: "True"

-DefaultLanguage
	Sets the default language selection
	OSDListOfLanguages.csv taken directly from ListOfLanguages.xml MDT "Scripts" folder
	ListOfLanguages.xml taken directly from MDT "Scripts" directory.
	Accepted values: Hyphenated language name e.g. "EN-US"

-DefaultTimezone
	Sets the default timezone
	Accepted values: Timezone ID name (see OSDTimeZonelist.csv) e.g. "Eastern Standard Time"

-AltCSVOUList
	Specify an alternate file name for the OU list CSV file
	Default value: "OSDOUList.csv"
	Accepted values: "YourOUListFilename.csv"

-AltCSVOSImages
	Specify an alternate file name for the OS images CSV file
	Default value: "OSDOSImages.csv"
	Accepted values: "YourOSImagesListFilename.csv"

-AltCSVApplications
	Specify an alternate file name for the OSD applications file
	Default value: "OSDApplications.csv"
	Accepted values: "YourApplicationsListFilename.csv"
	
-SkipCSVOUList
	Skips importing the OSDOUList.csv file
	Not needed if file does not exist
	Accepted values: "True"

-SkipCSVOSImages
	Skips importing OSDOSImages.csv file
	Not needed if file does not exist
	Accepted values: "True"

-SkipCSVApplications
	Skips importing OSDApplications.csv file
	Not needed if file does not exist
	Accepted values: "True"

-DisableTabComputerName
	Completely disable the Computer Name tab page and requirements
	Accepted values: "True"

-DisableTabAdminPassword
	Completely disable the Administrator Password tab page and requirements
	Accepted values: "True"

-DisableTabOperatingSystem
	Completely disable the Operating System tab page and requirements
	Accepted values: "True"

-DisableTabDeploymentReadiness
	Alias: -DisablePreflight
	Completely disable the Deployment Readiness preflight checks tab page
	NOTE: TPM check is automatically disabled if Win32_TPM SpecVersion is missing from WMI
	The checkbox is automatically checked if the "OSDImageName" is like *Win*10* regardless of TPM status
	Accepted values: "True"

-DisableTabLanguage
	Completely disable the Language tab page and requirements
	Accepted values: "True"

-DisableTabApplications
	Completely disable the Applications tab page and requirements
	Accepted values: "True"

-DebugWizard
	Hidden parameter
	Disables the error return for the TS invocation.
	Accepted values: "True"
```

## EXAMPLES
```
	Task sequence step as "Run PowerShell Script" with execution policy in Bypass
	Select the package with the script
	Script Name: OSD-Wizard-UDI-Replacement.ps1
	
	NOTE: Parameters must be set to "True", set as below, or not set. Any other option will cause a parameter validation error
	Parameters:
	-UDIWizardXMLFileName 'UDIWizard_Config.xml'
	-CustomWizardTitle 'CustomWizardTitle'
	-LockComputerName True
	-DisableComputerNameRequirement True
	-SerializeName True
	-NamePrefix 'NamePrefix'
	-DomainName 'DomainName'
	-DisableDomainName True
	-DisableOUSelection True
	-DisableUserName True
	-DisablePassword True
	-DisableBitlocker True
	-DefaultLanguage 'EN-US'
	-DefaultTimezone 'Pacific Standard Time'
	-AltCSVOUList 'YourOUListFilename.csv"
	-AltCSVOSImages 'YourOSImagesListFilename.csv'
	-AltCSVApplications 'YourApplicationsListFilename.csv'
	-SkipCSVOUList True
	-SkipCSVOSImages True
	-SkipCSVApplications True
	-DisableTabComputerName True
	-DisableTabAdminPassword True
	-DisableTabOperatingSystem True
	-DisableTabDeploymentReadiness True
	-DisableTabLanguage True
	-DisableTabApplications True
	-DebugWizard True
```

## OUTPUTS
```
	Files
	OSD_Wizard.log - OSD wizard selection output log
	Includes all output variables as well as device Make, Model, Serial Number, IP Address, and MAC Address information
	
	Task sequence variables that are set
	OSDComputerName
	OSDDomainName
	OSDDomainOUName
	OSDJoinAccount
	OSDJoinPassword
	OSDBuildAccount - Only returned if enabled and "-DisablePassword" selected
	OSDLocalAdminPassword
	OSDImageName - Legacy MDT variable for backward compatibility
	OSDDiskIndex - Physical disk number to be partitioned. Previous MDT/UDI variable was "OSDDiskIndex"
	OSDBitLockerMode - Only returns "True" if checked. Legacy MDT variable for backward compatibility
	OSDWindowsSettingsUILanguage
	OSDWindowsSettingsUserLocale
	OSDWindowsSettingsInputLocale
	OSDTimeZone
	UILanguage - Legacy MDT language variable for backward compatibility
	UserLocale - Legacy MDT language variable for backward compatibility
	KeyboardLocale - Legacy MDT language variable for backward compatibility
	CoalescedApps - Only returned if applications enabled
	Make - Device Manufacturer. Added to compensate for missing BDD.log
	Model - Device Model. Added to compensate for missing BDD.log
	SerialNumber - Device serial number. Added to compensate for missing BDD.log
	OSDWizardSuccess - Returns "False" by default. Returns "True" only upon wizard success. Previous MDT/UDI variable was "OSDSetupWizCancelled"
	OSDSetupWizCancelled - Returns "True" if wizard is cancelled. Legacy MDT language variable for backward compatibility
```

## NOTES
```
	Created  based on functionality of the MDT UDI wizard
	OSD wizard will only function in Windows PE unless "-DebugWizard True" is used. This will prevent the task sequence invocation from running
	OSD wizard will not run in a task sequence run from the Software Center due to limitations preventing PowerShell from interacting with the user
	Pre-flight checks taken directly from UDI wizard checks and Microsoft website
	https://learn.microsoft.com/en-us/archive/blogs/deploymentguys/pre-flight-checks-ac-power-check
	
	Local administrator user account creation (OSDAddAdmin variable) is not supported outside of the MDT and is intentionally omitted.
```

## LINKS
```
	Task sequence variable reference - https://learn.microsoft.com/en-us/mem/configmgr/osd/understand/task-sequence-variables
	Preflight checks - https://learn.microsoft.com/en-us/archive/blogs/deploymentguys/pre-flight-checks-ac-power-check
	UDI wizard task sequence variables - https://learn.microsoft.com/en-us/mem/configmgr/mdt/udi-reference#udi-task-sequence-variables
```

## UDI WIZARD XML INTEGRATED FUNCTIONALITY
```
	NOTE: Parameter input will take precedence over UDI wizard settings
	
-Required Files
	UDIWizard_Config.xml - UDI wizard XML configuration file must exist in the script root folder (Located in the MDT "Scripts" folder) or your custom UDI wizard .xml file
	UDIWizard_Config.xml.app - UDI wizard applications file must exist in the script root folder if applications are loaded from the XML (Located in the MDT "Scripts" folder)
	UDI_Wizard_Banner.bmp - UDI wizard banner file must exist in the script root folder (Located in the MDT Tools\x64 folder)
	Note: CSV files "OSD___.csv" are still recognized and will take precedence over existing UDI wizard XML settings. This is done to simplify the input of things like the OU and applications list
	Use one of the "-SkipCSV___" parameters to skip importing invididual CSV files
	
-UDI Wizard Pages
	Pages must be present in the UDI wizard Stage Group stage "NEWCOMPUTER". All other Stage Groups, including "NEWCOMPUTER.Prestaged", are ignored
	Each UDI wizard page type can exist only once under the Stage Group stage "NEWCOMPUTER"
	Pages not present in the UDI XML "NEWCOMPUTER" Stage Group will be disabled and removed from the OSD wizard.
	
-Only the below UDI wizard page types are recognized
	Microsoft.OSDRefresh.ComputerPage
	Microsoft.SharedPages.AdminAccountsPage
	Microsoft.OSDRefresh.VolumePage
	Microsoft.OSDRefresh.BitLockerPage
	Microsoft.OSDRefresh.ConfigScanPage
	Microsoft.OSDRefresh.LanguagePage
	Microsoft.OSDRefresh.ApplicationPage
	
	All field validators are ignored
	Only UDI wizard aspects that directly correspond to an aspect in the OSD wizard will be recognized. All other UDI wizard settings are ignored.
	All "Locked" and "Unlocked" field settings are recognized
	
-Page aspects that are recognized/used or not used 
	Microsoft.OSDRefresh.ComputerPage
	All fields recognized
	
	Microsoft.SharedPages.AdminAccountsPage
	Local administrator password field. Administrator account username is ignored
	Local administrator user account creation (OSDAddAdmin variable) is not supported outside of the MDT and is intentionally omitted.
	
	Microsoft.OSDRefresh.VolumePage
	Minimum Volume Size ignored
	User Data and Settings fields ignored
	
	Microsoft.OSDRefresh.BitLockerPage
	Only BitLocker checkbox is recognized (Initially check this checkbox in the wizard). All other aspects are ignored
	
	Microsoft.OSDRefresh.ConfigScanPage
	This only enables or disables the OSD wizard deployment readiness page
	Deployment readiness tab page (Microsoft.OSDRefresh.ConfigScanPage) validators are ignored.

	Microsoft.OSDRefresh.LanguagePage
	All fields recognized and used
	ListOfLanguages.csv is still required for the Language selection tab page
	Bug: Keyboard default language selection will be ignored if the XML keyboard local number (00000404) cannot be matched to a single existing language
	
	Microsoft.OSDRefresh.ApplicationPage
	Only Applications are recognized. Package/Programs are not recognized. (Possible support in a future version)
	Application groups are not recognized
	Application property mappings are ignored
```

## GENERAL FUNCTIONALITY
```
	OSD wizard will only function in Windows PE
	OSD wizard will not run in a task sequence run from the Software Center due to limitations preventing PowerShell from interacting with the user
	Use "-DebugWizard True" to allow functionality outside of Windows PE
	
	Logging
	Creates an "OSD_Wizard.log" file in the "_SMSTSLogPath" directory
	OSD_Wizard.Log logs the wizard output
		
	Wizard functionality and configurations
	Custom wizard Icon
	Include a ".ico" file in the script folder
	
	Custom wizard header/banner
	Include a ".bmp" file with the dimensions 759 x 69
	The UDI wizard banner listed in the XML file will take precedence over any other .bmp banner file
	NOTE: These are the same dimensions as the UDI wizard banner
	
	OU Selection dropdown list
	To populate the OU selection list
	Use the list from the UDI wizard XML
	Or, in the script folder include a CSV file named "OSDADOUList.csv" with a "Name" and "DistinguishedName" column
	Example:
	Name									DistinguishedName
	Sales Dept Workstations		OU=Workstations,OU=SalesDept,DC=Contoso,DC=Local
	NW Marketing Desktops		OU=Desktops,OU=Marketing,OU=NorthWestRegion,DC=Contoso,DC=Local
	
	OS Image selection dropdown list
	Use the list from the UDI wizard XML
	Or, in the script folder include a CSV file named "OSDOSImages.csv" with a "DisplayName" and "ImageName" column.
	When using multiple images, "ImageName" must match the logic for the "Apply Operating System Image" step matching the image name in the logic
	Items will appear in the dropdown list in the same order as the CSV
	Example:
	DisplayName							ImageName
	Windows 11 Enterprise 24H2		Windows 11 Enterprise 24H2 Base OS
	Windows 11 Enterprise 23H2		Windows 11 Enterprise 23H2 Base OS
	
	Language Selection page
	Use the list from the UDI wizard XML to set the default settings and lock the fields
	Items will appear in the dropdown list in the same order as the CSV
	OSDListOfLanguages.csv taken directly from ListOfLanguages.xml MDT "Scripts" folder
	To convert to CSV use this command
	([xml](Get-Content "\\Some\Pathto\OSDWizardRoot\ListOfLanguages.xml")).LOCALEDATA.LOCALE | ? { $_.SNAME -eq $_.SSPECIFICCULTURE } |
	select RefName, SNAME, SSPECIFICCULTURE, SENGDISPLAYNAME, DEFAULTKEYBOARD |
	ForEach-Object{
	[PSCustomObject]@{
	RefName		     = $_.RefName
	SNAME		     = $_.SNAME
	SSPECIFICCULTURE = $_.SSPECIFICCULTURE
	SENGDISPLAYNAME  = $_.SENGDISPLAYNAME
	DEFAULTKEYBOARD  = $_.DEFAULTKEYBOARD
	}
	} | Export-Csv "\\Some\Pathto\OSDWizardRoot\OSDListOfLanguages.csv" -NoTypeInformation -Force
	
	Use this command to generate an updated timezone list
	Items will appear in the dropdown list in the same order as the CSV
	Get-TimeZone -ListAvailable | Export-Csv -NoClobber -NoTypeInformation -Path "\\Some\Pathto\OSDWizardRoot\OSDTimeZonelist.csv"
	
	Applicaitons selection tab
	Items will appear in the dropdown list in the same order as the CSV
	Is empty by default
	To populate the Application selection list
	Use the list from the UDI wizard XML
	Or, in the script folder include a CSV file named "OSDApplications.csv" with the columns "DisplayName", "ApplicationName", "Required", and "Checked" columns
	Output creates "COALESCEDAPPS" variable for dynamic application installtion
	"ApplicationName" column is the applicaiton name of the app to be installed dynamically using
	"Required" column set to "True" will check all apps that are required installs. The apps can be unchecked, but will be rechecked when clicking "Next"
	"Checked" column set to "True" will check all apps that are default apps but are optional installs the user may uncheck
	Example:
	DisplayName				ApplicationName					Required	Checked
	Google Chrome			OSD - Chrome										TRUE
	Microsoft Office 365		OSD - Microsoft Office 365	TRUE
	Mozilla Firefox				OSD - Firefox										TRUE
	
	Task Sequence functionality and Configurations
	To run OSD Wizard, set task sequence step as "Run PowerShell Script" with execution policy in Bypass
	Set parameters as desired
	Wizard sets default task sequnce variable OSDWizardSuccess equals "False"
	OSDWizardSuccess is only set to "True" upon successfully completing the Wizard and clicking Finish
	When user cancelled "OSDSetupWizCancelled" returns "True". This is for MDT backward compatibility
	
	Format and Partition Disk task sequence step
	Variable name to store disk number should be set to "OSDDiskIndex"
	
	Enable BitLocker task sequence step
	NOTE: OSD wizard does not set Enable BitLocker step sub-settings, such as where to escrow the recovery key
	
	Task sequence step "Cancelled Wizard Group" logic
	TaskSequenceVariable OSDWizardSuccess equals "False"
	TaskSequenceVariable OSDSetupWizCancelled equals "True" (Legacy MDT variable left for backward compatibility)
```

## CHANGES
```
	Version 1.5
	Changed parameter -DisablePrefilght to -DisableTabDeploymentReadiness added alias DisablePrefilght
	Added options to disable and remove individual tab and requirements
	Added steps so "_SMSTSPackageName" and "_SMSTSOrgName" can be used to populate the custom wizard title
	Minor bug fixes and wording corrections
	Changed tab names variable for easier recogntion

	Version 1.6
	Added -DebugWizard parameter to allow wizard to run without invoking the TS environment

	Version 2.0
	Added tab for language selection support
	Change parameter -DisableComputerName to -LockComputerName
	Added -DisableComputerName alias
	Changed "OSDBitLockerMode" return variable to only return "True" when active. When "Enable BitLocker" is unchecked, "OSDBitLockerMode" variable is not populated or returned. This is to allow compatibility with existing task sequences
	Parameter "-DisabledBitlocker" will now lock the BitLocker check box preventing ir from being checked
	Removed OSDWizardSuccess result when clicking the Cancel button

	Version 2.1
	Changed memory check to pass 4GB when exactly 4GB memory is used - "4294967296" changed to (4GB - 64MB)
	Changed disk size check value - "68719476736" changed to 64GB
	Changed log output $NewOSDComputerName to $OSDComputerName to correct issue where the "OSDComputerName" variable was not correctly output to the log.

	Version 3.0
	Added confirm password field to Computer Name tab domain join credentials
	Added tab page to set the Local Administrator password
	Added parameter -DisableTabAdminPassword to disable the local administrator tab page
	Fixed BIOS serial number OSD variable. Change BIOSSerialNumber to BIOSSeralNumber
	Added log messages to indicate OSDJoinPassword and OSDLocalAdminPassword are being set. Passwords are [REDACTED]
	Minor wording changes and corrections
	Visual changes - added separators on some pages
	Added descriptive wording to wizard elements. Wording taken from the UDI wizard.

	Version 3.1
	Fixed bug that caused the TPM preflight check to run even when the TPM check was disabled causing a background error.

	Version 4.0
	Added support UDI wizard XML file
	Added parameters to specify alternate individual CSV file imports
	Set default CSV file names for import
	Added parameters to skip default individual CSV file imports
	OU selection combobox no longer disabled by default
	Added parameter to disable OU selection combobox
	Bitlocker checkbox no longer checked by default
	Update ListOfLanguages.csv file and generation script to include DefaultKeyboard column
	Fixed bug where password fields showed [REDACTED] on the finalization when no password was set
	Fixed bug where Finalization page would return an error if the applications list was enabled but no apps selected
	Changed computer name tab validation to check only if Computer Name textbox is enabled. Previous setting checked -DisableComputerNameRequirement
```
