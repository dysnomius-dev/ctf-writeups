# Emdee five for life


Emdee five for life, a web challenge on HTB

## What I did

The challenge started with a webpage where you need to submit a md5 hash. When you submitted the hash the website will tell you that you are to slow. I started with looking at the request that was send when trying to submit the form. I copied the request to python(with burp). After that I began to write a simple python script to get the string from the webpage, convert it to a hash and send it back. The code is in the repository and below:



```python
import requests
from lxml import html
import hashlib

burp0_url = "http://138.68.141.81:31468/"
burp0_cookies = {"PHPSESSID": "9av8gq6i7odn7j0i0bhdpc50q7"}
burp0_headers = {"Cache-Control": "max-age=0", "Upgrade-Insecure-Requests": "1", "Origin": "http://138.68.141.81:31468", "Content-Type": "application/x-www-form-urlencoded", "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9", "Referer": "http://138.68.141.81:31468/", "Accept-Encoding": "gzip, deflate", "Accept-Language": "en-US,en;q=0.9", "Connection": "close"}


testhash = 1
while True:
    burp0_data = {"hash": f"{testhash}"}
    r = requests.post(burp0_url, headers=burp0_headers, cookies=burp0_cookies, data=burp0_data)
    print(r.text)
    tree = html.fromstring(r.content)
    element = tree.xpath("/html/body/h3")
    string = element[0].text_content()
    testhash = hashlib.md5(string.encode('utf-8')).hexdigest()
```

It will print the HTML with the flag if you run it fast enough

## License
[MIT](https://choosealicense.com/licenses/mit/)
