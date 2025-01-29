# OSD-Wizard-UDI-Replacment
A PowerShell OSD wizard created based on the MDT UDI Wizard functionality

.SYNOPSIS
	OSD Wizard for SCCM Task Sequences

.DESCRIPTION
	OSD wizard replacement for the MDT UDI wizard.

.PARAMETER CustomWizardTitle
	Sets a custom title for the OSD Wizard

.PARAMETER DisableComputerName
	Disable the computername text box
	NOTE: Computer name field is still required by default. Use "-SerializeName" and/with "-NamePrefix" to ensure field is populated.
	You may also use -DisableComputerNameRequirement parameter

.PARAMETER DisableComputerNameRequirement
	Disables the requirement to enter text into the ComputerName textbox

.PARAMETER SerializeName
	Will compress and truncate all but the last 15 characters of the serial number.
	When used with "-NamePrefix", will truncate serial number so name is 15 characters
	NOTE: NamePrefix must be shorter than serial

.PARAMETER NamePrefix
	OPTIONAL. Sets the name prefix when using SerializeName. Can only be used with -SerializeName
	Example: NamePrefix = "DeptA-" then Computer name will be "DeptA-SerialNumber"
	NOTE: NamePrefix must be shorter than serial

.PARAMETER DomainName
	Sets the "OSDDomainName" domain name.
	If not set the Domain Name text box will be disabled

.PARAMETER DisableUserName
	Disable username text box.
	Leave enable to set "OSDJoinAccount" variable.
	NOTE: If 'DisablePassword" is set this will set "OSDBuildaccount" in place of "OSDJoinAccount". This is to allow a separate OSD join account

.PARAMETER DisablePassword
	Disable password text box
	Will set "OSDBuildaccount" in place of "OSDJoinAccount". This is to allow a separate OSD join account
	Leave enabled to set "OSDJoinPassword" variable

.PARAMETER DisableBitlocker
	BitLocker is checked by default.
	Uncheck BitLocker enabled checkbox.
	Leave enabled to set "OSDBitlockerMode" = "True" or "OSDBitlockerMode" = "True"

.PARAMETER DisablePreflight
	Disable preflight checks page
	NOTE: TPM check is automatically disabled if "OSDImageName" is like *Win*10* or Win32_TPM SpecVersion is missing from WMI

.EXAMPLE
	Task sequence step as "Run PowerShell Script" with execution policy in Bypass
	Select the package with the script
	Script Name: OSD_Wizard_Generic.Export.ps1
	
	NOTE: Parameters must be set to "True" or not set. Any other option will cause parameter a validation error
	Parameters:
	-CustomWizardTitle 'CustomWizardTitle'
	-DisableComputerName True
	-SerializeName True
	-NamePrefix 'NamePrefix'
	-DomainName 'DomainName'
	-DisableUserName True
	-DisablePassword True
	-DisableBitlocker True
	-DisablePreflight True

.OUTPUTS
	Files
	OSD_Wizard.log - OSD wizard selection output log
	
	Task sequence variables that are set
	OSDComputerName
	OSDDomainName - Only returned if enabled
	OSDDomainOUName - Only returned if enabled
	OSDJoinAccount - Only returned if enabled
	OSDJoinPassword - Only returned if enabled
	OSDBuildAccount - Only returned if enabled and "-DisablePassword" selected
	OSDImageName - Only returned if enabled
	OSDDiskIndex
	OSDBitLockerMode - Always returns "True" or "False"
	CoalescedApps - Only returned if applications enabled
	Make - Device Manufacturer
	Model - Device Model
	OSDWizardSuccess - Returns "False" by default. Returns "True" only upon wizard success

.NOTES
	Created  based on functionality of the MDT UDI wizard
	OSD wizard will only function in Windows PE
	Pre-flight checks taken directly from UDI wizard checks and Microsoft website
	https://learn.microsoft.com/en-us/archive/blogs/deploymentguys/pre-flight-checks-ac-power-check

.FUNCTIONALITY
	OSD wizard will only function in Windows PE
	
	Creates an "OSD_Wizard.log" file in the "_SMSTSLogPath" directory
	OSD_Wizard.Log logs the wizard output
	
	Wizard functionality and configurations
	Custom wizard Icon
	Include a ".ico" file in the script folder
	
	Custom wizard header/banner
	Include a ".bmp" file with the dimensions 759 x 69
	NOTE: These are the same dimensions as the UDI wizard banner
	
	OU Selection dropdown list
	Is disabled by default
	To populate the OU selection list
	In the script folder, include a CSV file named "OSDADOUList.csv" with a "Name" and "DistinguishedName" column
	Example:
	Name						DistinguishedName
	Sales Dept Workstations		OU=Workstations,OU=SalesDept,DC=Contoso,DC=Local
	NW Marketing Desktops		OU=Desktops,OU=Marketing,OU=NorthWestRegion,DC=Contoso,DC=Local
	
	OS Image selection dropdown list
	Is disable by default
	To populate the OS Images list, include a CSV file named "OSDOSImages.csv" with a "DisplayName" and "ImageName" column.
	When using multiple images, "ImageName" must match the logic for the "Apply Operating System Image" step matching the image name in the logic
	Example:
	DisplayName						ImageName
	Windows 11 Enterprise 24H2		Windows 11 Enterprise 24H2 Base OS
	Windows 11 Enterprise 23H2		Windows 11 Enterprise 23H2 Base OS
	
	Applicaitons selection tab
	Is disabled by default
	To populate the Application selection list
	In the script folder, include a CSV file named "OSDApplications.csv" with the columns "DisplayName", "ApplicationName", "Required", and "Checked" columns
	Output creates "COALESCEDAPPS" variable for dynamic application installtion
	"ApplicationName" column is the applicaiton name of the app to be installed dynamically using
	"Required" column set to "True" will check all apps that are required installs. The apps can be unchecked, but will be rechecked when clicking "Next"
	"Checked" column set to "True" will check all apps that are default apps but are optional installs the user may uncheck
	Example:
	DisplayName				ApplicationName				Required	Checked
	Google Chrome			OSD - Chrome							TRUE
	Microsoft Office 365	OSD - Microsoft Office 365	TRUE
	Mozilla Firefox			OSD - Firefox							TRUE
	
	Task Sequence functionality and Configurations
	To run OSD Wizard, set task sequence step as "Run PowerShell Script" with execution policy in Bypass
	Set parameters as desired
	Wizard sets default task sequnce variable OSDWizardSuccess equals "False"
	OSDWizardSuccess is only set to "True" upon successfully completing the Wizard and clicking Finish
	
	Format and Partition Disk task sequence step
	Variable name to store disk number should be set to "OSDDiskIndex"
	
	Enable BitLocker task sequence step
	Task sequence variable "OSDBitLockerMode" must be set to "True" or "False" in the Enable BitLocker task sequence step logic. (Default step logic is OSDBitLockerMode exists)
	e.g. TaskSequenceVariable "OSDBitLockerMode" equals "True" to run Enable BitLocker step
	NOTE: OSD wizard does not set Enable BitLocker step sub-settings such as where to escrow the recovery key
	
	Task sequence step "Cancelled Wizard Group" logic
	TaskSequenceVariable OSDWizardSuccess equals "False"
