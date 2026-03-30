[Link](https://app.hackinghub.io/hubs/smallmart)


Inspecting the source code,  I found two interesting authentication sections


Register function
```
@app.route("/register", methods=["GET", "POST"])
def register():
    if request.method == "POST":
        username = (request.form.get("username") or "").strip()
        password = (request.form.get("password") or "").strip()

        if not username or not password:
            flash("Username and password are required.", "error")
            return redirect(url_for("register"))

        if username.lower() == "admin":
            flash("This username is reserved.", "error")
            return redirect(url_for("register"))

        try:
            db = get_db()
            db.execute(
                "INSERT INTO users (username, password) VALUES (?, ?);",
                (username, password),
            )
            db.commit()
        except sqlite3.IntegrityError:
            flash("That username already exists (exact match).", "error")
            return redirect(url_for("register"))

        flash("Account created! Please login.", "success")
        return redirect(url_for("login"))

    return render_template("register.html", user=current_user())
```


This funcion checks whether the username is admin or not:

```
def is_admin_username(name: str) -> bool:
    return bool(re.match(r"^admin$", name or "", flags=re.IGNORECASE))
```
For the register function, basically, the application will take username and password then perform format string for those parameters, then it checks whether username field or password field is empty or not.
Also, it checks whether the username lowercase is equal to the `admin`

Furthermore, looking at the  `is_admin_username` function which compare with the regex `r"^admin$"`

`^` is the start of the string
`$` is the end of the string
By the way, notice that they set the `flag=re.IGNORECASE`. For example, `cat` and `Cat` will be the same

The main goal is registering an account which bypass the hurdel `username.lower() == "admin"` by using unicode dotless. Slightly modifying the character of `admin` to  `admın` which can set the flag to false in order to let us register an account.
Then accesssing `URL/admin` to retrieve the flag.

Happy Hacking!!!!
