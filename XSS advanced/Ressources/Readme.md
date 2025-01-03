# Breach: XSS advanced


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the media page (by clicking on the NSA logo) (`/?page=media&src=nsa`).

> When you try to edit the `src` parameter, you will notice that the src is rendered in a viewer.<br>
> You can use :
> * keywords (like `nsa`),<br>
> * relative urls (like `images/win.png`),<br>
> * plain data (like `data:text/html,<h1>Where is the flag?<h1>`),<br>
> We see that the src parameter is vulnerable to XSS.


#### Step 2:

Let's try to inject a script in the `src` parameter !

> First, let's try to inject a simple alert script.<br>
> ```url
> /?page=media&src=data:text/html,<script>alert('gimme the flag');</script>
> ```
> 
> It works !<br>
> But there is no flag for some reason.<br>
> Let's try to do the same thing, but with a base64 encoded script.

- Encode the following script in base64:
```html
<script>alert('gimme the flag');</script>
```

- It gives us:
```base64
PHNjcmlwdD5hbGVydCgnZ2ltbWUgdGhlIGZsYWcnKTs8L3NjcmlwdD4=
```


#### Step 3:

- Inject the base64 encoded script in the `src` parameter:
```url
/?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgnZ2ltbWUgdGhlIGZsYWcnKTs8L3NjcmlwdD4=
```


<br>


## How to fix it:

> The values of the parameters in an HTTP request are made by the user.

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

- Do more checks BEFORE trying to render or execute anything coming from the user.
  - Disallow the plain data (`data:__mime-type__;__encoding__,__data__`).
  - Or at least check for the mime-type (allow only `image/*` for example).


---

[[Back to main page](/#darkly)]
