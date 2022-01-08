# Django Cookies

```bash
HttpRequest.COOKIES - A dictionary containing all cookies. keys and values are strings.
```
#### Creating cookies
```bash
set_cookies() - set_cookie() is used to set/create/sent cookies.
```
> #### Syntax- 
`HttpResponse.set_cookies(key, value=' ', max_age = None, expires = None, 
 path = '/', domain = None, secure = False, httponly = False, samesite = None)`

```bash
set_cookie("name", "Saurabh")
```
- key - Name of the cookie
- value - Set the value of the cookie. This value is stored on the clients computer.

- max_age - It should be a number of seconds, or None(default) if the cookie should last only as long as the client's browser session. If expires is not specified, it will be calculated.

```bash
set_cookie("name", "Saurabh", max_age = 60*60*24*10)   //10 days
```
- expires - It should either be a string format `"Wdy,DD-Mon-YY HH:MM:SS GMT"` or a `datetime.datetime` object in `UTC`.

```bash
set_cookie("name", "Saurabh", expires = datetime.utcnow()+timedelta(days=2))
```
- path - Path can be /(root) or /mydir (directory)
```bash
set_cookie("name", "Saurabh","/") --> root

set_cookie("name", "Saurabh","/home")
```

- domain - Use domain if you want to set a cross-domain cookie. for eg. `domain="example.com"` will set a cookie that is readable by the domains `www.example.com`, `blog.example.com`, etc otherwise, a cookie will only be readable by the domain that set it.

```python
set_cookie("name", "Saurabh", domain="abc.com")
```
- secure - Cookie to only be transmitted over secure protocol as https. When it set to TRUE, the cookie will only be set if a secure connection exists.

```bash
set_cookie("name", "Saurabh", max_age=60*60*24*10, path="/", domain="abc.com", secure=True)
```

- httponly - use `httponly = True` if you want to prevent client-side Javascript from having access to the cookie.

> Httponly is a flag included in a set-cookie http response header. It's part of the RFC 6265 standard for cookies and can be a useful way to mitigate the risk of a client-side script accessing the protected cookie data.

```bash
set_cookie("name", "Saurabh", max_age=60*60*24*10, httponly=True)
```

- samesite - Use `samesite = 'Strict'` or `samesite = 'Lax'` to tell the browser not be send this cookie when performing a cross-origin request. Samesite isn't supported by all browsers, so it's not a replacement for Django's CSRF protection, but rather a defense in depth measure.

> 'RFC 6265' states that user agents should support cookies of at least 4096 bytes.


## Reading/Accessing Cookie
---

> #### Syntax- `request.COOKIES['key'];`
```bash
request.COOKIES['name'];
```
> #### Syntax- `request.COOKIES.get('key', default)`

```bash
request.COOKIES.get('name')
request.COOKIES.get('name', "Guest")
```
## Replace/Append Cookie
---

- key is same, it will replace
```bash
set_cookie("name","Saurabh")
set_cookie("name","Shubham")
```
- key is different, it will append
```bash
set_cookie("name","Saurabh")
set_cookie("firstname","Shubham")
```

## Deleting Cookie
---
```bash
HttpResponse.delete_cookie(key, path='/', domain = None)

delete_cookie('name')
```

### `views.py`
```python
def setcookie(request):
	response = render(request, 'app/setcookie.html')
	response.set_cookie('name', 'abc')
	return response

def getcookie(request):
	# nm = request.COOKIES['name']
    # nm = request.COOKIES.get('name')
    nm = request.COOKIES.get('name', "Guest")
	return render(request, 'app/getcookie.html', {'name':nm})

def deletecookie(request):
	response = render(request, 'app/deletecookie.html')
	response.delete_cookie('name')
```

## Creating Signed Cookies
---
> #### Syntax-
`HttpResponse.set_signed_cookie(key, value, salt='', max_age=None, expires=None, path='/', domain=None, secure=False, httponly=False, samesite=None)`

```python
response.set_signed_cookie('name','Saurabh', salt='nm', expires = datetime.utcnow()+timedelta(days=2))
```

## Getting Signed Cookies

`HttpRequest.get_signed_cookie(key, default = RAISE_ERROR, salt=' ', max_age = None)`

```python
nm = request.get_signed_cookie('name', salt='nm')

-- To display default value
nm = request.get_signed_cookie('name', default="Guest", salt='nm')
```