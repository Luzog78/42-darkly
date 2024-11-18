# Breach: XSS basic


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the feedback page.


#### Step 2:

- Enter one of the following name in the feedback form:
```html
s
sc
scr
scri
scrip
script
```


#### Step 3:

- Submit the form.


<br>


## How to fix it:

> Here it's clearly to to an `innerHTML` function :
> ```js
> element.innerHTML = content;
> ```
> 
> Huge error, because :

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

- Use the `innerText` to avoid script injection through the HTML :
  ```js
  element.innerText = content;
  ```


---

[[Back to main page](/#darkly)]
