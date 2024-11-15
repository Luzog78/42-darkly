# Breach: Include


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to any page but the index page

> We can see that the page is joined using the parameter `page` in the URL.<br>
> The value of the parameter may be a file name.<br>
> It means that we can possibly access files that are outside the web root directory.


#### Step 2:

There is a lot of sensitive files in a linux machine.<br>
One of them is the `/etc/passwd` file.<br>
Let's try to access it.

- We don't know where we are so we need to go back to the root directory.<br>
  We can do that by using multiple `../` in the URL.<br>
  Let's go back 20 times, just to be sure.

- And now, we can access the `/etc/passwd` file by using the following URL:
```
/?page=../../../../../../../../../../../../../../../../../../../../etc/passwd
```


<br>


## How to fix it:

- ...


---

[[Back to main page](/#darkly)]
