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
zeek readpcap pcaps/AsyncRAT.pcap zeek_logs/asyncrat
```

#### Analyze
```
rita import --logs ~/zeek_logs/asyncrat/ --database asyncrat
```


How many hosts are communicating with malhare.net?



Which Threat Modifier tells us the number of hosts communicating to a certain destination?



What is the highest number of connections to rabbithole.malhare.net?



Which search filter would you use to search for all entries that communicate to rabbithole.malhare.net with a beacon score greater than 70% and sorted by connection duration (descending)?




Which port did the host 10.0.0.13 use to connect to rabbithole.malhare.net?




