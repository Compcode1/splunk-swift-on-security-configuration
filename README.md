Splunk SwiftOnSecurity Configuration:
This project demonstrates how to enhance Windows endpoint visibility by integrating the Sysmon + SwiftOnSecurity configuration into a Splunk detection pipeline. The goal was to validate whether this high-fidelity telemetry setup can reliably detect obfuscated PowerShell execution and surface it clearly in both local logs and Splunk.

🔧 Project Objective
Upgrade default Windows telemetry using the community-recommended SwiftOnSecurity Sysmon config, then verify end-to-end visibility by:
Testing obfuscated PowerShell behavior
Confirming detection at the endpoint (Sysmon logs)
Validating successful ingestion and indexing in Splunk

📂 Project Structure
swift-on-security-config.ipynb
Jupyter Notebook that documents all actions, configuration changes, and validation steps taken during the project.

🛠️ Setup Summary
Install Sysmon on the Windows 11 host using the SwiftOnSecurity config.

Configure Splunk input to ingest:


Microsoft-Windows-Sysmon/Operational
via inputs.conf:


[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = main
renderXml = true


Restart Splunk to activate ingestion.
Run encoded PowerShell command that executes Notepad using:
powershell -EncodedCommand UwB0AGEAcgB0AC0AUAByAG8AYwBlAHMAcwAgAG4AbwB0AGUAcABhAGQALgBlAHgAZQA=


Validate telemetry locally with:
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | 
Where-Object { $_.Id -eq 1 -and $_.Message -like "*notepad.exe*" }



Validate in Splunk using:
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational" "notepad.exe"


✅ Result
Sysmon Event ID 1 (Process Create) captured the obfuscated PowerShell launch with full command-line detail.

Event was successfully ingested and indexed by Splunk, confirming visibility across both local and SIEM layers.

This demonstrates that SwiftOnSecurity’s config adds meaningful detection value and enhances response capability against stealthy attacker techniques.

📌 Notes
Future configurations may require validating other event types (e.g., file writes, registry, network) based on specific detection goals.

Splunk setup and permissions must be consistently re-verified during config changes to avoid ingestion failure.

