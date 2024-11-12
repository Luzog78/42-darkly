# Breach: Redirect


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the footer of the home page.

> There are three buttons of the page, each one redirecting to a different social media platform.


#### Step 2:

> The buttons are associated with a beacon <a> tag like this:
> ```html
> <a href="redirect.php?page=redirect&site=facebook"></a>
> ```

- Edit the `site` parameter in the href value of the beacon <a>.

- Example:
```html
<a href="redirect.php?page=redirect&site=flag"></a>
```


#### Step 3:

- Click on the button you just edited.


---

[[Back to main page](/#darkly)]
