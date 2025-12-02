# Concept 
Hunting around a Linux filesystem
# Method of Solving
I am given a note to find a passphrase with this creds
```
Access the user account:
username: eddi_knapp
password: S0mething1Sc0ming
```
Here are clues

1)
I ride with your session, not with your chest of files.
Open the little bag your shell carries when you arrive.
-> `.bash_history` or `env`
PASSFRAG1="3ast3r"

2)
The tree shows today; the rings remember yesterday.
Read the ledger’s older pages.
-> `git log -p` inside `.secret_git` directory
3)
When pixels sleep, their tails sometimes whisper plain words.
Listen to the tail.
-> `cat Pictures/.easter_egg`

# Complete passphrase
  => 3ast3r-1s-c0M1nG

# To access the note which is located on `/Documents` directory and change to `root` permission
```
gpg -d {file.gpg}
```

If you replace the contents of
/home/socmas/2025/wishlist.txt with this exact list (one item per line, no numbering),
the site will recognise it and the takeover glitching will stop. Do it — it will save the site.

Let's navigate to the wishlist `/home/socmas/2025/wishlist.txt` and modify the contents
with this

```text
Hardware security keys (YubiKey or similar)
Commercial password manager subscriptions (team seats)
Endpoint detection & response (EDR) licenses
Secure remote access appliances (jump boxes)
Cloud workload scanning credits (container/image scanning)
Threat intelligence feed subscription

Secure code review / SAST tool access
Dedicated secure test lab VM pool
Incident response runbook templates and playbooks
Electronic safe drive with encrypted backups
```

Then open FireFox browser in the VM -> The site will show a localhost on port 8080 then copy the Gibberish message
```
U2FsdGVkX1/7xkS74RBSFMhpR9Pv0PZrzOVsIzd38sUGzGsDJOB9FbybAWod5HMsa+WIr5HDprvK6aFNYuOGoZ60qI7axX5Qnn1E6D+BPknRgktrZTbMqfJ7wnwCExyU8ek1RxohYBehaDyUWxSNAkARJtjVJEAOA1kEOUOah11iaPGKxrKRV0kVQKpEVnuZMbf0gv1ih421QvmGucErFhnuX+xv63drOTkYy15s9BVCUfKmjMLniusI0tqs236zv4LGbgrcOfgir+P+gWHc2TVW4CYszVXlAZUg07JlLLx1jkF85TIMjQ3B91MQS+btaH2WGWFyakmqYltz6jB5DOSCA6AMQYsqLlx53ORLxy3FfJhZTl9iwlrgEZjJZjDoXBBMdlMCOjKUZfTbt3pnlHWEaGJD7NoTgywFsIw5cz7hkmAMxAIkNn/5hGd/S7mwVp9h6GmBUYDsgHWpRxvnjh0s5kVD8TYjLzVnvaNFS4FXrQCiVIcp1ETqicXRjE4T0MYdnFD8h7og3ZlAFixM3nYpUYgKnqi2o2zJg7fEZ8c=
```
  => UNLOCK_KEY: 91J6X7R4FQ9TQPM9JX2Q9X2Z
Then follow these commands
```
echo "{copied_text}" > /tmp/website_output.txt
openssl enc -d -aes-256-cbc -pbkdf2 -iter 200000 -salt -base64 -in /tmp/website_output.txt -out /tmp/decoded_message.txt -pass pass:'91J6X7R4FQ9TQPM9JX2Q9X2Z'
cat /tmp/decoded_message.txt
```
```text
Well done — the glitch is fixed. Amazing job going the extra mile and saving the site. Take this flag THM{w3lcome_2_A0c_2025}

NEXT STEP:
If you fancy something a little...spicier....use the FLAG you just obtained as the passphrase to unlock:
/home/eddi_knapp/.secret/dir

That hidden directory has been archived and encrypted with the FLAG.
Inside it you'll find the sidequest key.
```
with following comamnds
```
gpg  -o filename.tar.gz -d filename.tar.gz.gpg
gunzip filename.tar.gz
tar -xvf filename.tar
```
<img width="960" height="886" alt="Screenshot 2025-12-02 122148" src="https://github.com/user-attachments/assets/b2de6ab6-408d-4690-a27c-434cc7a49c46" />

Maybe Done
