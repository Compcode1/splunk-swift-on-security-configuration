Splunk SwiftOnSecurity Configuration: Enhanced Detection via Tuned Sysmon Telemetry
This project demonstrates how to upgrade Windows host visibility using Sysmon with the SwiftOnSecurity configuration, then validate detection of obfuscated PowerShell execution via local logs and Splunk SIEM. It builds directly on the Cybersecurity Battlefield framework by enhancing the Process Execution and Event Monitoring layers at the host level, while strengthening telemetry flow to the SIEM.

🎯 Objective
To replicate a common adversary technique — obfuscated PowerShell execution — and confirm that a professionally tuned Sysmon configuration provides high-fidelity telemetry from the endpoint to Splunk.

🔧 Battlefield Alignment
Layer	Purpose
Process Execution	Capture granular process creation and command-line arguments.
Event Monitoring	Improve detection fidelity with structured Sysmon logging.
SIEM Ingestion Layer	Route meaningful telemetry to Splunk and confirm indexing.

This tuning strategy reflects real-world SOC practice, where defenders prioritize signal-over-noise and tune telemetry to detect tactics such as Living-Off-the-Land (LOTL), persistence, and privilege escalation.

📂 Project Structure
swift-on-security-config.ipynb
Jupyter notebook documenting each configuration, execution, and validation step.

Telemetry Sources:

Sysmon logs (Event ID 1 – Process Create)

Splunk (ingesting Microsoft-Windows-Sysmon/Operational)

🛠️ Setup Summary
Sysmon Installation
Installed Sysmon on Windows 11.

SwiftOnSecurity Configuration
Downloaded and applied sysmonconfig-export.xml from SwiftOnSecurity GitHub.

sh
Copy
Edit
Sysmon64.exe -c sysmonconfig-export.xml
Splunk Configuration
Configured inputs.conf to ingest Sysmon logs:

ini
Copy
Edit
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = main
renderXml = true
Test Attack Simulation
Ran obfuscated PowerShell to launch Notepad:

powershell
Copy
Edit
powershell.exe -EncodedCommand UwB0AGEAcgB0AC0AUAByAG8AYwBlAHMAcwAgAG4AbwB0AGUAcABhAGQALgBlAHgAZQA=
✅ Results
Stage	Outcome
Sysmon Logging	Full visibility of encoded command-line, process parent, integrity level, and hashes.
Splunk Ingestion	Event correctly indexed in Splunk with expected sourcetype and searchability.
Detection Confirmation	Validated using local PowerShell queries and Splunk searches.

🔍 Detection Summary
Technique Simulated: Obfuscated PowerShell Execution

Telemetry Captured:

Full command-line

Parent process relationship

Integrity level and Logon ID

Process hashes and GUIDs

Event ID Logged: Sysmon Event ID 1

Splunk Query Example:

spl
Copy
Edit
index=main sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" "notepad.exe"
📌 Strategic Value
This project shows how curated telemetry tuning enhances visibility without overwhelming analysts. It reflects enterprise SOC practices where raw audit verbosity is replaced by structured signal, enabling detection of:

Obfuscated execution

LOLBins and native tool abuse

Process chains supporting attack narratives

Persistence attempts via scheduled tasks

Reconnaissance and lateral movement indicators

🧠 Skills Demonstrated
Endpoint telemetry tuning and validation

Splunk configuration and event ingestion

Obfuscation detection strategy

SIEM query design

Host-level detection engineering


