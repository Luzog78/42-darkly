# Breach: Redirect


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the footer of the home page.

> There are three buttons of the page, each one redirecting to a different social media platform.


#### Step 2:

> The buttons are associated with a beacon &lt;a&gt; like this:
> ```html
> <a href="redirect.php?page=redirect&site=facebook"></a>
> ```

- Edit the `site` parameter in the href value of the beacon &lt;a&gt;.<br>
  Example:
```html
<a href="redirect.php?page=redirect&site=flag"></a>
```


#### Step 3:

- Click on the button you just edited.


<br>


## How to fix it:

> The flag appears before we can see if there is a "breach".<br>
> So there is nothing to fix in this case.

> But we can see that the server has a `redirect` page, taking a `site` parameter.<br>
> We can imagine that the algorithm that chooses the correct social pages<br>
> redirects us to the value of `site` with `www.` in front and `.com/42born2code` at the end.<br>
> This could possibly represent a security issue (it depends on the implementation).

- The things to keep in mind when dealing with the values of a parameter are :
  - They may have been edited by the user
  - Beware of possible injections and memory overloads.
  - Always checks everything, in this case :
    - Check that the string is exactly `facebook` or `instagram`,
    - If it is not, don't redirect and display an error.


---

[[Back to main page](/#darkly)]
