# Breach: Cookies


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Open the developer tools and go to the cookies page.

- You'll notice that there is a cookie named `I_am_admin` with the value `68934a3e9455fa72420237eb05902327`.

- This value is the md5 hash of the string `false`.

#### Step 2:

- Find the md5 hash of the string `true` --> `b326b5062b2f0e69046810717534cb09`.

- Change the value of the cookie `I_am_admin` to `b326b5062b2f0e69046810717534cb09`.

#### Step 3:

- Refresh the page.


<br>


## How to fix it:

> The cookies are local and can be edited by the user at any moment.

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

- So DO NOT put sensitive content in the cookies !<br>
  The rights of a user/session should be done server-side.


---

[[Back to main page](/#darkly)]
