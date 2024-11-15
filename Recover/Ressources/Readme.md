# Breach: Recover


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the "Sign in" page and click on the "Forgot password?" link.

Welcome to the account recovery page.

> You notice only a button to submit your recovery request.<br>
> But when you click on the button, you get an error message.


#### Step 2:

- Inspect the button.

> You can see that the button is part of a form.<br>
> Inside the form, you can see a hidden input field :
> ```html
> <input type="hidden" name="mail" value="webmaster@borntosec.com" maxlength="15">
> ```

- Edit the value of the hidden input field.<br>
  Example:
```html
<input type="hidden" name="mail" value="flag" maxlength="15">
```


#### Step 3:

- Submit the form.


<br>


## How to fix it:

- ...


---

[[Back to main page](/#darkly)]
