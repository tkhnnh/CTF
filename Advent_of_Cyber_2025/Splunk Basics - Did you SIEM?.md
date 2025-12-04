# Log Analysis with Splunk
## Exploring the logs
<img width="1364" height="792" alt="66b36e2379a5d0220fc6b99e-1732792280322" src="https://github.com/user-attachments/assets/adfddd13-cb0f-4e18-9b7d-87291a048c94" />



<img width="1884" height="694" alt="5e8dd9a4a45e18443162feab-1762172345961" src="https://github.com/user-attachments/assets/f625f8b1-55aa-4254-b131-73f5eded2cd9" />

1. Search bar
2. Filter?
3. Selection for filter?

 Be presented with two separate datasets that have been pre-ingested into Splunk.

<img width="1105" height="666" alt="5e8dd9a4a45e18443162feab-1761870512197" src="https://github.com/user-attachments/assets/2654a514-98bd-452e-ae82-0216c443ef55" />

Two datasets are as follows:
- `web_traffic`: This data source contains events related to web connections to and from the web server.
- `firewall_logs`: This data source contains the firewall logs, showing the traffic allowed or blocked. The local IP assigned to the web server is `10.10.1.15`.

## Intitial Triage 
<img width="1889" height="876" alt="5e8dd9a4a45e18443162feab-1761870512197" src="https://github.com/user-attachments/assets/cd4d233a-2a9b-4b90-afda-8a07fde82f19" />

1. Search query
2. Time range
3. Timeline -> visual histogram distributtion of the events over time => The graph indicates the successful daily log volume followed by a distinctive traffic spike (a period of high activity, likely the attack window)
4. Selected fields -> displayed in the summary of the event list (`host`, `source`, `sourcetype`) -> representing basic metadata about the log file itself
5. Interesting fields -> fields prefixed with `#`
6. Event details & field extraction

## Visualizing the Logs Timeline
2 Types of visualizing:
- Statistics
- Visualization

`reverse` function in the search query at the end to display the result in descending order

## Anomaly Detection

### User Agent
`user_agent` field, apart from legitimate user agents like Mozilla's variants -> a large number of suspicious ones
<img width="1095" height="784" alt="5e8dd9a4a45e18443162feab-1762375952691" src="https://github.com/user-attachments/assets/3300204a-2455-43a4-b031-5360b1fe9147" />


### client_ip
suspicious ip connected
### path
Path traverse attack or enumerating

## Filtering out Benign Values
exact value should be in the form key=/key!= *<value>*

## Narrowing Down Suspicious IPs
`sort -count` will sort the result by count in reverse order
## Tracing the Attack Chain
### Reconnaisance (Footprinting)
Searching for the initial probing of exposed configuration files 
```
sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time, path, user_agent, status
```
### Enumeration (Vulnerability Testing)
Search for path traversal and open redirect vulnerabilities
```
sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..*" OR path="*redirect*"
```

```
sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..\/..\/*" OR path="*redirect*" | stats count by path
```

### SQL Injection Attack
```
sourcetype=web_traffic client_ip="<REDACTED>" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status
```
## Exfiltration Attempts
Search for attempts to download large, sensitive files (backups, logs)
```
sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent
```

## Ransomware Staging & RCE
Requests for sensitive archives like `/log.tar.gz` and `/config`

## Correlate Outbound C2 communication
search to `firewall_logs` using the Compromised Server IP

## Volume of Data Exfiltrated
use the sum function to calculate the sum of the bytes transferred
```
sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip
```


### What is the attacker IP found attacking and compromising the web server?
### Which day was the peak traffic in the logs?
### What is the count of Havij user_agent events found in the logs?
### How many path traversal attempts to access sensitive files on the server were observed?
### Examine the firewall logs. How many bytes were transferred to the C2 server IP from the compromised web server?
