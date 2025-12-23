Real Intelligence Threat Analytics (RITA) is an open-source framework created by Active Countermeasures.

## Feature

- C2 beacon detection
- DNS tunneling detection
- Long connection detection
- Data exfiltration detection
- Checking threat intel feeds
- Score connections by severity
- Show the number of hosts communicating with a specific external IP
- Shows the datetime when the external host was first seen on the network

RITA only accepts network traffic input as `Zeek logs`

=> Zeek is an open-source network security monitoring (NSM) tool -> observes network traffic via configured SPAN ports (used to copy traffic from one port to another for monitoring), physical network taps, or imported packet captures in the PCAP format

Zeek can convert packet capture into zeek structured logs
```
zeek readpcap ~/pcaps/rita_challenge.pcap zeek_logs/asyncrat1
```

#### Analyze
```
rita import --logs ~/zeek_logs/asyncrat1/ --database asyncrat
```

Now that RITA has parsed and analyzed our data, we can view the results by entering the command `rita view <database-name>`
 

How many hosts are communicating with malhare.net?
`6`


Which Threat Modifier tells us the number of hosts communicating to a certain destination?
`prevalence`


What is the highest number of connections to rabbithole.malhare.net?
`40`

Which search filter would you use to search for all entries that communicate to rabbithole.malhare.net with a beacon score greater than 70% and sorted by connection duration (descending)?

<img width="1918" height="580" alt="Screenshot_2025-12-23_16-28-44" src="https://github.com/user-attachments/assets/57fd5e5f-58d2-4821-8194-4cceb12ce009" />



Which port did the host 10.0.0.13 use to connect to rabbithole.malhare.net?

<img width="1920" height="504" alt="Screenshot_2025-12-23_16-30-33" src="https://github.com/user-attachments/assets/ec411adc-7a7d-48ae-8bb8-7a0332bf550d" />



