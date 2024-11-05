# Breach: Cookies


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Open the developer tools and go to the cookies page

- You'll notice that there is a cookie named `I_am_admin` with the value `68934a3e9455fa72420237eb05902327`

- This value is the md5 hash of the string `false`

#### Step 2:

- Find the md5 hash of the string `true` --> `b326b5062b2f0e69046810717534cb09`

- Change the value of the cookie `I_am_admin` to `b326b5062b2f0e69046810717534cb09`

#### Step 3:

- Refresh the page


---

[[Back to main page](/#darkly)]
