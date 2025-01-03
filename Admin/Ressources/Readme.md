# Breach: Admin


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


Let's try to access a hidden page !


#### Step 1:

- Install `gobuster` if you haven't already.

- Run the following command to find hidden pages:
```bash
gobuster dir -u "http://$IP/" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

> You will find some hidden pages like `/css`, `/js`, `/fonts`, `/images`...<br>
> But two of them are interesting: `/admin` and `/whatever`.


#### Step 2:

- Access the `/whatever` page.

> It's a file explorer page.


#### Step 3:

- Find and download the file named `htpasswd`.

> The content is :
> ```
> root:437394baff5aa33daa618be47b75cb49
> ```

- `437394baff5aa33daa618be47b75cb49` is the md5 hash of the string `qwerty123@`.


#### Step 4:

- Go to the `/admin` page.


#### Step 5:

- Enter the following credentials:
  - Username: `root`
  - Password: `qwerty123@`


<br>


## How to fix it:

- Do not allow file and directory listing on your server.<br>
  This could never be a good idea.<br>
  Or at least restrict the listing pages to certain users, not every random guy.

<br>

- Never store passwords in plain text (or even with a weak hash method).<br>
  And in no case the file containing sensitive info should be accessible by a random user.

<br>

- <u>Recommendation:</u><br>
  Use a stronger password and something else than `root` for an admin account.


---

[[Back to main page](/#darkly)]
