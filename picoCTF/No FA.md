<img width="1919" height="944" alt="image" src="https://github.com/user-attachments/assets/6faa962f-4232-4e67-a1c8-0dd8d09da66d" />Link : [No FA](https://learn.cylabacademy.org/library/765?page=1&category=1&workspace=true&difficulty=2)
Type : White Box
Level : Medium

With the provided `users.db`,
Using `sqlite3` to view the database
```
sqlite3 users.db
sqlite3> .tables
users
sqlite3> SELECT * FROM users;
1|john.doe|john.doe@nfa.com|599a4410e2af69d1585f16d82d4b5f0abf3ad09fa42b9d55d7b7a50671ccf8c1|0
2|jane.smith|jane.smith@nfa.com|81c68634d1b211e0d5632839f7efc8601c743f1ef0c94da8220e26ab221efff1|0
3|robert.jones|robert.jones@nfa.com|aaf120fcb16e20e2d18e63e668e060b5e4a52c5e0b3f038777365fe87ca2ccdb|0
4|emily.brown|emily.brown@nfa.com|9e85668a071a595fe9222725bfb591cdaa0d880e3a7c7de1d9ddd3d4b7d08772|0
5|admin|iamadmin@nfs.com|c20fa16907343eef642d10f0bdb81bf629e6aaf6c906f26eabda079ca9e5ab67|1
6|michael.davis|michael.davis@nfa.com|576454d8921440f30609200a7f79073ec5b69ee284f27bbb860620d56416ad94|0
7|linda.wilson|linda.wilson@nfa.com|082a6006d9c87749adff6be260461171b508744a90a45f75abe78d92995485c5|0
8|david.garcia|david.garcia@nfa.com|faa32a09d4798d21486344a140fd0977cbec33fd5b045bca83c04efb364c49d9|0
9|jennifer.rodriguez|jennifer.rodriguez@nfa.com|c1488b6d9ed8352a64f979506583f33d80aa4119190f7892bc481e8984c880d0|0
10|christopher.williams|christopher.williams@nfa.com|0bf3a14c03e9c7034b9588a69f828840fd32bd739c37b613f41c4aecee26e277|0
11|angela.martinez|angela.martinez@nfa.com|e64b5893827166e4568af8ece105d8c0839772ae10fba3c11e77b5fb3c0ef0c6|0
12|kevin.anderson|kevin.anderson@nfa.com|8bac48021ebd453dbd876d43fa28c8e383fc16176fc8b12fa474b01eb9fa4df5|0
13|melissa.thomas|melissa.thomas@nfa.com|564c89c28d93e8485b76a41deca21ab28e60a32c506e479b925f4643722e9f83|0
14|brian.jackson|brian.jackson@nfa.com|7fccba2f216750414443626058128539ef5a8859f7cb20da2b22d8d787ec6fc2|0
15|stephanie.white|stephanie.white@nfa.com|64acea3bdefef67d65e6a36ee66ac66e85d39931639ea926d1fc98fedd28905b|0
16|eric.harris|eric.harris@nfa.com|b9590eaeaa25401398ebd4b98e10182f4e265f396f23a11eb8fdb18d66a1685c|0
17|michelle.martin|michelle.martin@nfa.com|9b68124e23f3bb700682d28d1d750bec95794a193097b59526ef038f810cb34c|0
18|patrick.thompson|patrick.thompson@nfa.com|1549f62e486c006cbbacee5947c3f6815a0c5f3ef54c80f1f0b17c2ae9da5866|0
19|nicole.garrett|nicole.garrett@nfa.com|5647517c88d64c95170fdb734dc22ba45e284f219d1266eb14f4d9dd7a099ce3|0
20|joseph.cole|joseph.cole@nfa.com|49a57175de704a0ec2a006746d20d375814581bb35552ce0a0b13683426fd232|0
```

The most noticable one is line 5th
```
5|admin|iamadmin@nfs.com|c20fa16907343eef642d10f0bdb81bf629e6aaf6c906f26eabda079ca9e5ab67|1
```

Using crackstation to crack the hash
<img width="999" height="292" alt="Screenshot 2026-07-12 225616" src="https://github.com/user-attachments/assets/03d52211-980f-4cf3-a9ed-98df49bc2c39" />

Then use that to login `admin:apple@123`
<img width="1919" height="944" alt="Screenshot 2026-07-12 231121" src="https://github.com/user-attachments/assets/ded63f35-15f0-430b-b98b-3966c111626b" />



```
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        user = db.get_user_by_username(username)
        if user and hashlib.sha256(password.encode()).hexdigest() == user['password']:
            if user['two_fa']:
                # Generate OTP
                otp = str(random.randint(1000, 9999))
                session['otp_secret'] = otp
                session['otp_timestamp'] = time.time()
                session['username'] = username
                session['logged'] = 'false'
                # send OTP to mail ---
                return redirect(url_for('two_fa'))
            else:
                session['username'] = username
                session['logged'] = 'true'
                flash('Login successful!', 'green')
                return redirect(url_for('home'))
        else:
            flash('Invalid username or password', 'red')
    return render_template('login.html')
```
-> after login, we would receive the cookie which part of it contains the OTP code, by decoding the cookie can help me identify the right OTP. Since base on the pattern that i have seen so far, this is flask framework
-> [tool](https://www.kirsle.net/wizards/flask-session.cgi)
Go to Inspect -> Application tab -> Cookies
<img width="1898" height="946" alt="Screenshot 2026-07-12 232610" src="https://github.com/user-attachments/assets/cc2bc89e-301b-4f79-9e85-b3b17285b405" />

<img width="1037" height="946" alt="Screenshot 2026-07-12 232659" src="https://github.com/user-attachments/assets/4769d0ff-a236-49a3-947c-62979092c461" />


Happy Hacking!@#!@
