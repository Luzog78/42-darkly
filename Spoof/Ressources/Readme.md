# Breach: Spoof


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Goto the albatroz page (at the bottom of the home page, by clicking on the copyright notice).


#### Step 2:

- View the source code of the page.

> You will find 2 interesting comments :
> ```html
> <!-- You must come from : "https://www.nsa.gov/". -->
> ```
> ```html
> <!-- Let's use this browser : "ft_bornToSec". It will help you a lot. -->
> ```


#### Step 3:

- Open BurpSuite, refresh and intercept the HTTP request.


#### Step 4:

- Change the `Referer` header to `https://www.nsa.gov/`.

- Change the `User-Agent` header to `ft_bornToSec`.


#### Step 5:

- Forward the request.


<br>


## How to fix it:

> The data inside the requests are user-side and can be edited by the user at any moment.<br>
> Making some pages accessible depending on the `User-Agent` is stupid.<br>
> It's not because the `User-Agent` is `Mozilla` that the user does not use Chrome.

##### RULE N°1 IN SECURITY: NEVER TRUST THE USER !

- Do not base sensitive things on the content of a simple request.


---

[[Back to main page](/#darkly)]
