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

> The HTML, the CSS and even the JavaScript are local and can be edited by the user at any moment.<br>
> Having a `type="hidden"` to hide somthing from the user is useless.

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

> I did not really get the point of this flag...<br>
> But if the goal was to send the recovery to `webmaster@borntosec.com` in every case :

- Do the important things server-side !<br>
  Do not take the e-mail from the html form, the user could have modified it.


---

[[Back to main page](/#darkly)]
