#Powershell Profile and Environment

This is a collection of scripts, personal profile preferences that I pieced together from people much smarter (or at least more developer minded) than I. I'm also trying to include my PowerShell development environment settings like fonts and apps but this is secondary. This is all for a personal need to be able to setup a standardized environment between my work and home PCs (or rebuild it quickly in either).

##Profile Description
I found and repurposed Joel Bennett's work for much of this part. His work is genius and I'm probably doing it an extreme disservice but I made several small changes and simplifications to it where I ran into issues. I repurposed is environment.psm1 module as a general location for profile related functions (and fixed some path mangling issues I ran into for some reason). But truthfully 95% of the best parts of this environment are Joel's scriptcraft.

###Profile Configuration
There are 2 major components of the profile. There is an environment.psm1 file that includes the lion's share of important functions for the profile. This includes Joel's 'Set-Prompt' function that is used to retain command history across sessions among other things.

I've also included a number of one off scripts that are not used in the actual profile but are part of my personal essential scripts or used in the initial configuration of the profile. This includes:
- Connect-ExchangeOnline.ps1
- Disconnect-ExchangeOnline.ps1
- Load-PowerCLI.ps1
- Load-Vagrant.ps1
- Remove-ScriptSignature.ps1
- Set-ProfileScriptSignature.ps1
- New-CodeSigningCertificate.ps1

The profile directory tree for my configuration looks like this:

`C:\Users\<user id>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`

`C:\Users\<user id>\Documents\WindowsPowerShell\Scripts\*.ps1`

`C:\Users\<user id>\Documents\WindowsPowerShell\Modules\Environment\Environment.psm1`

`C:\Users\<user id>\Documents\WindowsPowerShell\Data\quotes.txt`

###Installing
All you need to do is copy the contents of this repo into your WindowsPowerShell directory then optionally create a self-signed code signing certificate and run a script signing script I put together. I put together a quick install.ps1 file to do this but if I were you I'd just do this manually so you know nothing is getting accidentially overwritten.

`iex (New-Object Net.WebClient).DownloadString("https://raw.githubusercontent.com/zloeber/PowerShellProfile/master/Install.ps1")`

Once you have installed the basic components you can secure your profile a bit with a code signing certificate. An extra script or two I put together will do just that for ya if you like:

`. (Split-Path $Profile)\Scripts\New-CodeSigningCertificate.ps1`

`. (Split-Path $Profile)\Scripts\Set-ProfileScriptSignature.ps1`

`Set-ExecutionPolicy AllSigned`

The script will look for a code signing certificate and attempt to create one if it doesn't already exist. It is suggested to go ahead and do this if you aren't using one already to sign your scripts. This will give you a few prompts as it tries to move the self-signed certificate to the appropriate store to be trusted.

Assuming that the self-signed code signing certificate gets created or already exists the next script will try to sign the environment.psm1 and Microsoft.PowerShell_profile.ps1 scripts to help prevent tampering of your profile. This really only becomes effective if you also set your local system to have its execution policy of AllSigned. Check your execution policy with this:

`Get-ExecutionPolicy -List`

If you change your profile script or the Environment.psm1 file just run the following to re-sign them again:
`. (Split-Path $Profile)\Scripts\Set-ProfileScriptSignature.ps1`

Note: After the profile loads, it will set the Process scoped execution policy to RemoteSigned!


##Other Information
**Author:** Zachary Loeber

**Website:** http://www.the-little-things.net

**Github:** https://github.com/zloeber/PowerShellProfile

##Other credits:
[HarooPad](http://pad.haroopress.com/)
[Joel Bennett](http://http://huddledmasses.org/)