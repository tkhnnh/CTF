# Initial Recon
## About the target
<img width="1016" height="799" alt="Screenshot_2025-09-09_22-42-24" src="https://github.com/user-attachments/assets/cb768fc6-6e38-44ce-9be0-c32e8cddd930" />
The web interface presented NAND Simulator with a nice GUI which the players can reset the game, add nodes, connect and submit in order to get the answer.

## First Attempt
There is a given link to the [server](https://challenge-files.picoctf.net/c_activist_birds/7eac27979c12e4bd449f03e40a8492044221b7d2a96ac85f1150e30983c56eac/server.tar.gz) folder
I viewed all the contents inside that folder which misguided and confused me a lot
Then I reckon that I need to use other methods rather than sticking my eyes to these codes

## Intercept response
While intercepting the POST reponse of the submit the node function, I accidentally got lucky and the web revealed the flag for me.
<img width="1920" height="929" alt="Screenshot_2025-09-09_23-03-16" src="https://github.com/user-attachments/assets/0ced7e64-af06-4f64-ac31-56b6edcae3e2" />
However, I still tried to figure our how to actually achieve it and I continued my work.
I tried to connect nodes like this image below then submit to get the reponse and a magical thing appeared
<img width="1028" height="753" alt="Screenshot_2025-09-09_23-10-17" src="https://github.com/user-attachments/assets/fb6f2080-4b20-49db-9378-f20db9f2f1e4" />
<img width="1042" height="310" alt="Screenshot_2025-09-09_23-12-06" src="https://github.com/user-attachments/assets/162e4991-d0c0-4e3e-969c-c03425045ada" />
I reckoned that I needed to brutefoce these values of each key to find the 200 which contain the flag

I had to fuzz the value of the Node using Burpsuite Intruder
<img width="515" height="420" alt="Screenshot_2025-09-09_23-15-16" src="https://github.com/user-attachments/assets/a510c41d-2589-41c9-a672-6a3217f45624" />
<img width="819" height="54" alt="Screenshot_2025-09-09_23-15-24" src="https://github.com/user-attachments/assets/58ac36ac-8bfe-4c32-8c98-54c00ecbef57" />
Tips to avoid overwhelming bruteforce instances
- longest lenght
- code 200
And viewing the flag only need to access the response section
<img width="1665" height="16" alt="Screenshot_2025-09-09_23-26-57" src="https://github.com/user-attachments/assets/9b6fecb4-d136-4d99-9ec7-dc3bc0476b0b" />
