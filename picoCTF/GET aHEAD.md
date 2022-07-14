# _GET aHEAD_
## Description
Find the flag being held on this server to get ahead of the competition `http://mercury.picoctf.net:15931/`
## Solution
View page source, notice that there are two buttons on the page, `Choose Red` & `Choose Blue`, one uses `Get` method and one uses `Post`
```html
<!doctype html>
<html>
<head>
    <title>Red</title>
    <link rel="stylesheet" type="text/css" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
	<style>body {background-color: red;}</style>
</head>
	<body>
		<div class="container">
			<div class="row">
				<div class="col-md-6">
					<div class="panel panel-primary" style="margin-top:50px">
						<div class="panel-heading">
							<h3 class="panel-title" style="color:red">Red</h3>
						</div>
						<div class="panel-body">
							<form action="index.php" method="GET">
								<input type="submit" value="Choose Red"/>
							</form>
						</div>
					</div>
				</div>
				<div class="col-md-6">
					<div class="panel panel-primary" style="margin-top:50px">
						<div class="panel-heading">
							<h3 class="panel-title" style="color:blue">Blue</h3>
						</div>
						<div class="panel-body">
							<form action="index.php" method="POST">
								<input type="submit" value="Choose Blue"/>
							</form>
						</div>
					</div>
				</div>
			</div>
		</div>
	</body>
</html>
```
Intercept the requests, notice that both request didn't submit any data to the webserver, the only difference is the request `method` I assume the webserver determines the respond by request methods

![image](https://user-images.githubusercontent.com/70738420/178637759-5c9b4c46-2355-4128-b3f9-742f39dd627c.png)

![image](https://user-images.githubusercontent.com/70738420/178638106-2874fbdb-90d1-45ad-bba3-1edd06fba850.png)

The challenge name gives a hint, Get a`HEAD`, request with method `HEAD` to retrieve the flag 
`curl` with `-I` to get the flag, `-I` uses the `HEAD` method, which only gets the header

```console
❯ curl -I http://mercury.picoctf.net:15931/
HTTP/1.1 200 OK
flag: picoCTF{r3j3ct_th3_du4l1ty_82880908}
Content-type: text/html; charset=UTF-8
```


