# Breach: File upload


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the upload page.

> You can upload a file, but only jpeg files are allowed.<br>
> You ask yourself how the server knows the type of the file you upload.

> So you try to upload a file with a different extension.<br>
> It it works, it means that the server only checks the extension of the file.<br>
> You would just have to change the mime type of the file to `text/html` or `application/javascript`<br>
> and hope that the image is "rendered" directly to execute the code.<br>
> <br>
> Spoiler, it does not work.

> So maybe the server checks the mime type of the file.<br>
> In this case, you'll be able to upload a file with a different extension !<br>
> Even if the mime type is `image/jpeg`, when the server will render the file,<br>
> it will execute the code inside !


#### Step 2:

Let's try to edit the mime type of the file you upload.

- Get yourself a request editor (like Burp Suite, Postman or the developer tools of Firefox).

- Send a request and intercept it.


#### Step 3:

- Edit the name of the file you upload.<br>
  You want to execute some code so you can name it `script.js` or `whatever.php` for example.

The body request should look like this:
```
...
Content-Disposition: form-data; name="uploaded"; filename="some_script.js"
Content-Type: image/jpeg
...
```


#### Step 4:

- Forward the request.


<br>


## How to fix it:

> Here, only data with `image/jpeg` as mime-types was allowed.<br>
> But we know the rule :

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

- Check everything !
  - The extension
  - The mime-type
  - The size
  - And even if you want, the content:
    - Either the first bytes to be sure that it's a jpeg header
    - Or the full data with a jpeg-checker/safe-jpeg-parser package.

<br>

- Every check should be done <b>at least</b> server-side.


---

[[Back to main page](/#darkly)]
