# Detect Suspicious Web commands

signs of command execution attempts, such as cmd.exe, PowerShell, or Invoke-Expression
With this query on Splunk
```text
index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status
```

After decode the powershell base64 string

```
echo "VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA" | base64 -d
This is now Mine! MUAHAAHAA
```
# Looking for Server-Side Errors or Command Execution in Apache Error Logs
```text
index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
```
this query looks for any file with above string and those executable files triggered the message



What is the reconnaissance executable file name?
`whoami.exe`
What executable did the attacker attempt to run through the command injection?
`powershell.exe`
