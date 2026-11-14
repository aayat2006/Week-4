# TryHackMe Writeup: Splunk: Exploring SPL
Platform:TryHackMe  
Internship:DG Interns Hub  
Week:Week 4 
Date Completed:11/09/2026  
Status:Completed

# Task 1: Introduction
I am ready to to explore Splunk's query language!
No answer needed

# Task 2: Search & Reporting
Submit your first query for All Time: index=windowslogs.
How many total events do you see?
12256

After you submit your first query, look in the Fields sidebar.
Which SourceIP has recorded the most amount of events?
172.90.12.11


How many events appear on 04/15/2022 from 08:05 AM to 08:06 AM?
134


# Task 3: Search Operators
How many events in the windowslogs index have an EventID field value equal to 4624?
26

How many events are observed with the DestinationIp = 172.18.39.6 and DestinationPort = 135?
4


Use the query index=windowslogs Hostname=Salena.Adam DestinationIp=172.18.38.5
Which SourceIp returns the highest count?
172.90.12.11


How many events are returned when you search the term cyber*?
12256

Which operator is given the lowest priority in Splunk searches?
AND


# Task 4: Filtering Results
Use the fields command to highlight Domain, SourceProcessId, and TargetProcessId.
Which SourceProcessId has the highest value?
9496

Try out this query index=windowslogs | regex TargetObject="Manager$".
Which TargetObject field value contains the highest number of results?
HKLM\SOFTWARE\Microsoft\SecurityManager


# Task 5: Structuring Results
Build a table that highlights the EventID, AccountName, and AccountType fields.
Which AccountName appears first in your results?
SYSTEM

Append the above query to include the reverse command.
Which EventID appears first?
800

Use the query below to build a timeline of events.
What password was given to the user A1berto?

index=windowslogs EventID=1
| table _time ParentProcessId ProcessId ParentCommandLine CommandLine
| reverse
paw0rd1


# Task 6: Transforming Commands
Use the top command to query the Image field.
Which Image field value has the most occurrences?
C:\windows\system32\svchost.exe

Try out the iplocation command with the SourceIp field.
Which Region do the IP addresses in your events originate from?
California

Try out this lookup query. Which Image field value has the highest RiskScore?

index=windowslogs
| lookup image_riskscore Image OUTPUT RiskScore
| stats count by Image RiskScore
| sort - RiskScore

C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe


# Task 7: Anomaly Detection
Run the first anomaly detection query (index=vpnlogs).
Which other user is marked as an outlier?
jsmith


Which country is anomalous for the user from Q1?
Answer Example: CN
JP

Run the second anomaly detection query (index=vpnlogs).
Which user suspiciously logged in at 3 AM?
njackson


# Task 8: Conclusion
Complete the room and continue on your cyber learning journey!

No answer needed

