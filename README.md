# PowerShell-Interview
Document Resources, Topics, Questions and Tips for a PowerShell Development Interview

Many of the topics/questions may not come directly in your interview, but it would be a good idea to understand them, in order to understand PowerShell better. Which will give you an edge in the interview and leave a better impression if you explain it in details.

# Questions & Topics

## What is PowerShell?

* PowerShell is a shell designed especially for system administrators.
* **Open Source** and **Platform Independent (Windows/Linux/Mac)**
* **Object oriented**, not text-based
* Built on .NET framework
* Interactive prompt and a scripting environment.

<hr>

## How does powershell differ from other scripting languages
TBD
<hr>

## PowerShell versions and differences
TBD
<hr>

## Execution Policies

### Types of Execution Policy?

There are 6 types of execution policies

#### 1. Restricted 
This is the default. PowerShell will not run any script, including PowerShell profiles.

#### 2. RemoteSigned
PowerShell will run any script that you create locally. But any script that has been detected as coming from the Internet, such as via Internet Explorer, Microsoft Outlook, Mozilla Firefox or Google Chrome must be digitally signed with a code signing certificate that is trusted by the computer.

#### 3. AllSigned
PowerShell will not run any script unless it has been digitally signed with a trusted code signing certificate.

#### 4. Unrestricted
PowerShell will make no attempts to hinder script execution and will run any script. If the script comes from an untrusted source, like the Internet, you will be prompted once to execute it. Though it is not preferred.

#### 5. Bypass
There is also a Bypass policy, which I don’t recommend for daily use. This policy will run any script without question or prompting. The assumption is that you have taken steps outside of Nothing is blocked and there are no warnings or prompts.PowerShell to verify the safety and integrity of the script.

#### 6. Undefined
There is no execution policy set in the current scope. If the execution policy in all scopes is Undefined, the effective execution policy is Restricted, which is the default execution policy.

### What is the order in which execution policy is evaluated ?

Windows PowerShell determines the effective policy by evaluating the execution policies in the following precedence order -

1. **Group Policy**: Computer Configuration
2. **Group Policy**: User Configuration
3. **Execution Policy**: Process (or PowerShell.exe -ExecutionPolicy) - CURRENT SCOPE
4. **Execution Policy**: CurrentUser  - SAVED in HKCU registry
5. **Execution Policy**: LocalMachine – SAVED in HKLM registry
<hr>

## CIM vs WMI

|Old WMI| New WMI |CIM|
|--|--|--|
|Stands for **Windows Management Instrumentation**|Stands for **Windows Management Instrumentation**| Stand for **Common Information Model**|
|Old WMI is Microsoft's initial implementation of CIM|New WMI was released with WMF v3 in 2012 which was compliant to new CIM standards| Vendor-neutral, **industry standard way of representing management information**|
|Developed by MicroSoft|Developed by MicroSoft|Developed by the DMTF|
|Since PowerShell v1|Introduced in **PowerShell v3**|
|Microsoft used **DCOM (Distributed COM) / RPCs (Remote Procedure Calls)**|Uses WSMan and no more DCOM errors|Uses WSMan, a standard developed by DMTF|
|Windows only|Windows only|Any platform|
|Get-WMIObject|**Get-CimInstance, Get-CimClass, Invoke-CimMethod**|No cmdlets|
|More or less deprecated and you're **connected to LIVE objects and can play with them**| **Not connected** to LIVE objects, stateless relationship with the remote machine||

### Old WMI

* Uses DCOM/RPC. Uses old-style native code providers and a repository. 
* Available only on Windows. 
* More or less deprecated, meaning it's not a focus area for further improvement or development. You're connected to "live" objects and can play with them.

### New WMI

* Uses WS-MAN (via WinRM service). Supports old-style native code providers and a repository, as well as new-style MI providers. 
* Available only on Windows. 
* The way forward. If something can talk to "NEW WMI" it should be able to talk to OMI, also. You're not connected to "live" objects, and have an essentially stateless relationship with the remote machine.

### OMI

* Uses WS-MAN (OMI code includes the protocol stack). Supports only new-style MI providers. 
* Available on any implementing platform. Also the way forward. If something can talk to OMI, it should be able to talk to "NEW WMI" also.

### CIM

* Defines the standard. Created by DMTF. 
* Early versions were implemented as "OLD WMI" by Microsoft, newest version implemented both in "NEW WMI" and OMI by Microsoft and others.


### Area of confusion

In 2012 with Windows Management Framework 3, Microsoft releases a new version of WMI. **They fail to give it a unique name**, which causes a lot of confusion, but it complies with all the latest CIM specifications. 

The PowerShell cmdlets that uses this new WMI has CIM in their noun part of the cmdlet, like **Get-CimInstance, Get-CimClass, Invoke-CimMethod** **But these aren't  CIM** because CIM isn't a protocol. They're talking WS-MAN, which is what the new CIM standard specifies.

<i> Credits: 

* https://powershell.org/2015/04/24/management-information-the-omicimwmimidmtf-dictionary/#.W3BpB-gzaUk 
* https://richardspowershellblog.wordpress.com/2013/03/24/wmi-vs-cim/
</i>
<hr>

## WinRM and WSMan and DCOM

### WSMan

* WS-Management or **Web Services-Management** is a DMTF (**Distributed Management task force**)
* It is an open standard defining a SOAP-based (**Simple Object Access Protocol**) protocol for the management of servers, devices, applications and various Web services.
* **Vendor Neutral**, common way for **systems to access and exchange management information** across the IT infrastructure.

### WinRM

* Microsoft has implemented the WS-Management standard in **Windows Remote Management (WinRM)**.
* WinRM is a feature of Windows Vista that allows administrators to remotely run management scripts. 
* It handles remote connections by means of the WS-Management Protocol, which is based on **SOAP (Simple Object Access Protocol)**. 

### DCOM

* DCOM stands for **Distributed COM (Component Object Model)**
* Used to connect LIVE objects on the remote machine. 
* That meant you could get a WMI instance, execute methods, change properties. 
* The RPC protocol was designed for that kind of continuous back-and-forth. 
* But it is **network/memory inefficient** due to LIVE objects

<hr>

## Automatic variables

* Describes variables that store state information for PowerShell. 
* These variables are created and maintained by PowerShell.

Some very common Automatic Variables

**$$** - Contains the last token in the last line received by the session.

**$?** - Contains the execution status of the last operation. It contains TRUE if the last operation succeeded and FALSE if it failed.

**$^** - Contains the first token in the last line received by the session.

**$_** - Same as $PSItem. Contains the current object in the pipeline object. You can use this variable in commands that perform an action on every object or on selected objects in a pipeline.

**$Args** - Contains an array of the undeclared parameters and/or parameter values that are passed to a function, script, or script block. When you create a function, you can declare the parameters by using the param keyword or by adding a comma-separated list of parameters in parentheses after the function name.

**$Error** - Contains an array of error objects that represent the most recent errors. The most recent error is the first error object in the array ($Error[0]).

**$ForEach** - Contains the enumerator (not the resulting values) of a ForEach loop. You can use the properties and methods of enumerators on the value of the $ForEach variable. This variable exists only while the ForEach loop is running; it is deleted after the loop is completed. For detailed information

**$Home** - Contains the full path of the user's home directory. This variable is the equivalent of the %homedrive%%homepath% environment variables, typically C:\Users\<UserName>.

**$OFS** - $OFS is a special variable that stores a string that you want to use as an **output field separator** . Use this variable when you are converting an array to a string. By default, the value of $OFS is " ", but you can change the value of $OFS in your session, by typing $OFS="<value>". If you are expecting the default value of " " in your script, module, or configuration output, be careful that the $OFS default value has not been changed elsewhere in your code.

**$PID** - Contains the process identifier (PID) of the process that is hosting the current Windows PowerShell session.

**$Profile** - Contains the full path of the Windows PowerShell profile for the current user and the current host application. You can use this variable to represent the profile in commands. For example, you can use it in a command to determine whether a profile has been created

Know more:
```PowerShell
Get-Help about_Automatic_Variables
```

<hr>

## Parameter binding
    * By Value
    * By PropertyName
<hr>

## Powershell Pipelines

Pipeline processing

<i> Credits: 
* https://www.red-gate.com/simple-talk/sysadmin/powershell/ins-and-outs-of-the-powershell-pipeline/
* https://www.red-gate.com/simple-talk/dotnet/.net-tools/down-the-rabbit-hole--a-study-in-powershell-pipelines,-functions,-and-parameters/ </i>

<hr>

## Powershell Scopes

* Global, Local, Script, Private
* https://ss64.com/ps/syntax-scopes.html

<hr>

## Powershell Workflows

* https://blogs.technet.microsoft.com/heyscriptingguy/2012/12/26/powershell-workflows-the-basics/

<hr>


## Powershell adaptive systems
TBD

<hr>

## Creating methods of an object
TBD

<hr>

## What has been most challenging work you have done. 
TBD

<hr>

## Out-Host, Write-Output, Write-Host
TBD

<hr>

## Number of ways to create an object

```PowerShell
# 1. Using Hashtables
[pscustomobject]@{
    firstname = 'Prateek'
    lastname =  'Singh'
}

# 2. Using Select-Object
Select-Object @{n='firstname';e={'Prateek'}},@{n='lastname';e={'Singh'}} -InputObject ''

# 3. Using New-Object and Add-memeber
$obj = New-Object -TypeName psobject
$obj | Add-Member -MemberType NoteProperty -Name firstname -Value 'Prateek'
$obj | Add-Member -MemberType NoteProperty -Name lastname -Value 'Singh'

# 4. Using New-Object and hashtables
$properties = @{
    firstname = 'Prateek'
    lastname = 'Singh'
}		
$o = New-Object psobject -Property $properties; $o
```

<hr>

## How to rename a variable
TBD

<hr>
    	
## Return vs write-output
TBD


<hr>

## Modules vs Snap-ins
TBD

<hr>

## What is a Filter?
TBD

<hr>

## How to reverse order of a String

```PowerShell
# Method 1
$a='String'.tochararray();  [array]::Reverse($a)

# Method 2
$a = '';for($i=$($Str.Length-1);$i -ge 0;$i--){$a+=$Str[$i]} ; $a

# Method 3
$str[$($str.Length-1)..0] -join ''
```

<hr>

## How to save credentials in your PowerShell Scripts

* The use ConvertTo-SecureString and ConvertFrom-SecureString **without a Key or SecureKey, Powershell will use Windows Data Protection API (DPAPI) to encrypt/decrypt** your strings. 

* This means that it will **only work for the same user on the same computer**.

* Using **a Key/SecureKey, the AES encryption algorithm** is used that allows you to use the stored credential from any machine with any user so long as you know the AES Key that was used.

```PowerShell
$user = "UserName"
$password = 'Password@123'| ConvertTo-SecureString -AsPlainText -Force | ConvertFrom-SecureString
$Creds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, ($password | ConvertTo-SecureString)
```

<hr>

## How to take Passwords input from users in a secure way?

```PowerShell
Read-Host -AsSecureString
```

<hr>

## 	What is cryptographic algorithm used in ConvertTo-SecureString ?
1. AES  - Advanced Encryption Standard 
2. DPAPI - WIndows **Data Protection API** is used to encrypt your strings

<i> Credits: 
* https://powershell.org/2014/02/01/revisited-powershell-and-encryption
* https://powershell.org/2013/11/24/saving-passwords-and-preventing-other-processes-from-decrypting-them/#.W3FsA-gzaUk
 </i>

<hr>

## Explain what is the function of $input variable?
TBD

<hr>

## What is $_ and $PSItem variable
TBD

<hr>

## What are two ways of extending PowerShell?
    PSSnapins
    Modules
<hr>

## You have a script which uses Read-Host to prompt the user for an IP address. You need to make sure the user inputs a valid IP address. How would you do that ?

1. Splitting the address in 4 elements and try to cast them to a [byte]
2. A regular expression [regex]
3. Cast the input string to the [System.Net.IPAddress] class

<hr>

## Advanced Functions		

* [cmdletbinding()]
* Begin{} process{} end {} blocks 

https://ss64.com/ps/syntax-function-input.html

<hr>

## CredSSP issues in PowerShell and workarounds

### Double Hop Issue

PowerShell remoting to connect to Server-1 which then attempts to connect from Server-1 to Server-2 but the second connection fails, this is a Double Hop issue.

Because, PSRemoting authenticates via **Network Logon** which works by showing possession of the credential, but since remote server doesn’t have the credential, it fails! the second Hop Server-1 to Server-2.



### Workaround

* PowerShell provides the CredSSP option which performs **“Network Clear-text Logon”** instead of a **“Network Logon”**. 
* CredSSP Network Clear-text Logon sends clear-text password to the remote Server-1 in clear-text, which eventually is used to authenticate to Server-2, in the second hop. 

```PowerShell
# On client machine, from where you do the PowerShell Remoting
Enable-WSManCredSSP –Role Client –DelegateComputer Server2.ridicurious.com -Force

# Checking
Get-WSManCredSSP

# On Server
Enable-WSManCredSSP –Role Server -Force

# Usage - Make sure to use CredSSP Authentication
Enter-PSSession –ComputerName Server2.ridicurious.com –Credential RidiCurious\administrator –Authentication CredSSP
```

### CAUTION

* This is not constrained delegation. CredSSP passes the user's full credentials to the server without any constraint.
* And if the Server is compromised, attackers can even read your credentials in plain-text using tools like **Mimikatz**

### What is CredSSP

* The **Credential Security Support Provider protocol** (CredSSP) is a Security Support Provider that is implemented by using the Security Support Provider Interface (SSPI)
* CredSSP lets an **application delegate the user's credentials from the client to the target server** for remote authentication. 
* CredSSP provides an encrypted **Transport Layer Security Protocol channel** (TLS). The client is authenticated over the encrypted channel by using the Simple and Protected Negotiate (SPNEGO) protocol with either Microsoft Kerberos or Microsoft NTLM.


![](https://github.com/PrateekKumarSingh/PowerShell-Interview/blob/master/images/DoubleHop_CredSSP.png)

<i> Credits
* https://docs.microsoft.com/en-us/windows/desktop/secauthn/credential-security-support-provider
* https://www.powershellmagazine.com/2014/03/06/accidental-sabotage-beware-of-credssp/
</i> 

<hr>

## PowerShell Remoting ( PSRemoting )

### Architecture

![](https://github.com/PrateekKumarSingh/PowerShell-Interview/blob/master/images/PSRemoting.png)

<i> Credits: https://github.com/devops-collective-inc/secrets-of-powershell-remoting/blob/master/manuscript/remoting-basics.md </i>


### How to enable PSRemoting on a server?

#### Server Side        
```PowerShell
Enable-PSRemoting -Force

# The asterisk is a wildcard symbol for all PCs. If instead you want to restrict computers that can connect, you can replace the asterisk with a comma-separated list of IP addresses or computer names for approved PCs.

Set-Item wsman:\localhost\client\trustedhosts *
        
# After running that command, you’ll need to restart the WinRM service so your new settings take effect. Type the following cmdlet and then hit Enter:

Restart-Service WinRM
```

#### Client Side

```PowerShell
Set-Item wsman:\localhost\client\trustedhosts *
```
#### Testing the PSRemoting

```PowerShell
Test-WSMan
```

or, you can run `Get-PSSessionConfiguration` cmdlet to see the PowerShell configurations

```
PS C:\> Get-PSSessionConfiguration


Name          : microsoft.powershell
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users
                AccessAllowed

Name          : microsoft.powershell.workflow
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : microsoft.powershell32
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users
                AccessAllowed

```

### Remoting Returns Deserialized Data
* The results you receive from a remote computer have been **serialized into XML**, and then **deserialized on your computer**. 

* In essence, the objects placed into your shell's pipeline are static, detached snapshots of what was on the remote computer at the time your command completed. 

* These **deserialized objects lack the methods of the originals objects**, and instead only offer static properties.

* If you need to access methods or change properties, or in other words if you must **work with the live objects**, simply make sure you do so **on the remote side**, before the objects get serialized and travel back to the caller


### What is Implicit remoting?

![](https://github.com/PrateekKumarSingh/PowerShell-Interview/blob/master/images/ImplcitRemoting.png)

```PowerShell
PS C:\> $s = New-PSSession -ComputerName Server01
PS C:\> Import-Module -PSSession $s PSWorkflow
PS C:\> Get-Module

ModuleType Name                ExportedCommands
---------- ----                ----------------
Manifest  Microsoft.PowerShell.Management   {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Content...}
Manifest  Microsoft.PowerShell.Utility    {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Script   PSScheduledJob           {Add-JobTrigger, Disable-JobTrigger, Disable-ScheduledJob, Enable-Job...
```

The proxy commands look like the real commands, but **they're functions, NOT Cmdlets.**
```PowerShell
PS C:\> Get-Command -Module PSScheduledJob

CommandType   Name                        ModuleName
-----------   ----                        ----------
Function    Add-JobTrigger                PSScheduledJob
Function    Disable-JobTrigger            PSScheduledJob
Function    Disable-ScheduledJob          PSScheduledJob
Function    Enable-JobTrigger             PSScheduledJob
Function    Enable-ScheduledJob           PSScheduledJob
Function    Get-JobTrigger                PSScheduledJob
Function    Get-ScheduledJob              PSScheduledJob
Function    Get-ScheduledJobOption        PSScheduledJob
Function    New-JobTrigger                PSScheduledJob
Function    New-ScheduledJobOption        PSScheduledJob
Function    Register-ScheduledJob         PSScheduledJob
Function    Remove-JobTrigger             PSScheduledJob
Function    Set-JobTrigger                PSScheduledJob
Function    Set-ScheduledJob              PSScheduledJob
Function    Set-ScheduledJobOption        PSScheduledJob
Function    Unregister-ScheduledJob       PSScheduledJob
```


<hr>

## Try, Catch, Finally
TBD

<hr>

## Errors
    * terminating, non-terminating errors
    * throw
    * Write-error    
    * $ErrorActionVariable, -ErrorAction parameter


## SOAP and REST API


|SOAP|REST|
|--|--|
|SOAP stands for **Simple Object Access Protocol**| REST stands for **Representational State Transfer**|
|SOAP has been around a while|REST is a newcomer and fixes few problems with SOAP|
|Relies exclusively on XML|Can also use other smaller message/data formats like JSON, CSV or even RSS|
|Have to use XML for requests by making RPC calls, and response if received in XML as well|REST relies on a simple URL in many cases|
|Slow, requires bandwidth|Fast - lighter weight alternative|
|In-Built error handling| NA |
|SOAP is **a protocol**. SOAP was designed with a specification. It includes a **WSDL (Web Service Definition Language)** file which has the required information on what the web service does in addition to the location of the web service.| REST is an Architectural style in which a web service can only be treated as a RESTful service if it follows the constraints of being Client ,Server, Stateless, Cacheable, Layered System, Uniform Interface|
|SOAP cannot make use of REST since SOAP is a protocol and REST is an architectural pattern.|REST can make use of SOAP as the underlying protocol for web services, because in the end it is just an architectural pattern.|
|SOAP uses service interfaces to expose its functionality to client applications. In SOAP, the WSDL file provides the client with the necessary information which can be used to understand what services the web service can offer.|REST use Uniform Service locators to access to the components on the hardware device.|
|SOAP requires more bandwidth for its usage. Since SOAP Messages contain a lot of information inside of it, the amount of data transfer using SOAP is generally a lot.|REST does not need much bandwidth when requests are sent to the server. REST messages mostly just consist of JSON messages. Below is an example of a JSON message passed to a web server. You can see that the size of the message is comparatively smaller to SOAP. **{"city":"Mumbai","state":"Maharastra"}**|
|Tranfer on HTTP, FTP and SMTP etc| Only HTTP|



