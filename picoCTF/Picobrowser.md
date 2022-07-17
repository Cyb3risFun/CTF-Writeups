# **_Picobrowser_**
## Description
This website can be rendered only by picobrowser, go and catch the flag! `https://jupiter.challenges.picoctf.org/problem/26704/`
## Solution
`curl` with `user-agent` set as `picobrowser`
```console
❯ curl -s -H "User-Agent:picobrowser" https://jupiter.challenges.picoctf.org/problem/26704/ | html2text

    * Home
    * Sign_In
    * Sign_Out
**** My New Website ****
Flag
© PicoCTF 2019
```
Can't see the flag itself, but there is a `Flag` text in the page, lets see what it is
```html
❯ curl -s -H "User-Agent:picobrowser" https://jupiter.challenges.picoctf.org/problem/26704/
<!DOCTYPE html>
<html lang="en">

<head>
    <title>My New Website</title>

    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">

    <link href="https://getbootstrap.com/docs/3.3/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet">

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

</head>

<body>

    <div class="container">
        <div class="header">
            <nav>
                <ul class="nav nav-pills pull-right">
                    <li role="presentation" class="active"><a href="#">Home</a>
                    </li>
                    <li role="presentation"><a href="/unimplemented">Sign In</a>
                    </li>
                    <li role="presentation"><a href="/unimplemented">Sign Out</a>
                    </li>
                </ul>
            </nav>
            <h3 class="text-muted">My New Website</h3>
        </div>

        <!-- Categories: success (green), info (blue), warning (yellow), danger (red) -->


        <div class="jumbotron">
            <p class="lead"></p>
            <p><a href="/flag" class="btn btn-lg btn-success btn-block"> Flag</a></p>
        </div>


        <footer class="footer">
            <p>&copy; PicoCTF 2019</p>
        </footer>

    </div>
    <script>
    $(document).ready(function(){
        $(".close").click(function(){
            $("myAlert").alert("close");
        });
    });
    </script>
</body>

</html>%
```
Turns out its a link
> <p><a href="/flag" class="btn btn-lg btn-success btn-block"> Flag</a></p>

Vist the link with same `user-agent` set and grab the flag
```console
❯ curl -s -H "User-Agent:picobrowser" https://jupiter.challenges.picoctf.org/problem/26704/flag | html2text

    * Home
    * Sign_In
    * Sign_Out
**** My New Website ****
×  picobrowser!
Flag: picoCTF{p1c0_s3cr3t_ag3nt_e9b160d0}

© PicoCTF 2019
```



