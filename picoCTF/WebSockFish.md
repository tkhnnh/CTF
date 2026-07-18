Resouce: [WebSockFish](https://learn.cylabacademy.org/library/480?page=2&category=1&difficulty=2)

Category: Web Exploitation

Difficulty: Medium

The platform is a chess game.
<img width="513" height="573" alt="Screenshot 2026-07-18 103011" src="https://github.com/user-attachments/assets/79d8163c-7a9c-45d1-8a7c-e4805f22bf8a" />

Reading the javascript source code in the `Page Source`, The stockfish mecahnism will execute the snapback whenever I try to promote the pawn, which automatically makes best move. This way, I could win but could not retrieve the flag.

However, capturing the request of websocket, whenever player move 
<img width="1815" height="687" alt="Screenshot 2026-07-18 103803" src="https://github.com/user-attachments/assets/c1e976d1-fcc2-4229-af04-cdd3a07905f9" />

Then I try to modify the value with lower number and retrieve the flag
<img width="1852" height="820" alt="Screenshot 2026-07-18 103746" src="https://github.com/user-attachments/assets/1b59848f-28d1-4da3-a836-0e31ea7e9211" />


Happy Hacking@!$@#!
