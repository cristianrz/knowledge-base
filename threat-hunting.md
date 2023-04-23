# Threat Hunting

## Windows

### What to look for

* Did the expected parent process spawn it?
* Is it running out of the expected path?
* Is it spelled correctly?
* Is it running under the proper SID?
* Is it signed my Microsoft?

### Processes

* `smss.exe`: session manager, only 1 should be running
* `csrss.exe`:
	* Client Server Run Subsystem Process
	* malware might misspel it
	* typically 2 instances should be running
* `winlogon.exe`:
	* Windows Logon Process
* `wininit.exe`: only 1 instance
* `lsm.exe`: local session manager
	* after windows 7 it is now `lsm.dll`
* `services.exe`: only 1 instance
* `lsass.exe`: Local Security Authority Subsystem
	* only 1 instance