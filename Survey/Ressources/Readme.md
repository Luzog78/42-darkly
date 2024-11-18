# Breach: Survey


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the survey page.


#### Step 2:

- Open the developer tools and edit any value of the survey form to more than 10.

Example:
```html
<!-- Change -->
<option value="1">1</option>
<!-- To -->
<option value="1000">1</option>
```


#### Step 3:

- Submit the form


<br>


## How to fix it:

> The HTML, the CSS and even the JavaScript are local and can be edited by the user at any moment.<br>
> So because the `value` field is user-side, we have to add server-side checks !

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

- Add server-side checks.<br>
  Example :<br>
  ```py
  try:
      safe_value = int(value)
      if 0 < safe_value <= 10:
          # safe
          # ...
          # ...
      else:
          pass  # out of bounds
  except:
      pass  # not a number
  ```


---

[[Back to main page](/#darkly)]
