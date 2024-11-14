# Breach: Bruteforce


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the sign in page (either `/index.php?page=signin` or `/?page=signin`).

> When you try to log in, the form redirect you to the page :<br>
> `/?page=signin&username=_____&password=_____&Login=Login`<br>

> You notice also that an image is displayed, saying "WrongAnswer".<br>
> You can deduce that when the login is correct, the image "WrongAnswer" will not be displayed.


#### Step 2:

Let's try to bruteforce a login. We will use Hydra to do so.

- Install `hydra` if you haven't already.


#### Step 3:

Let's get the list of most common usernames and passwords.

- You can find a list of ~3.5k common passwords in `/usr/share/wordlists/john.lst`.
  - This list is native to Kali Linux.
  - But you can find it on the official repo of John The Ripper.<br>
    [john-the-ripper/run/password.lst](https://github.com/pmittaldev/john-the-ripper/blob/master/run/password.lst).

- The repo SpecLists gives a quite exhaustive list of usernames.<br>
  [SecLists/Usernames/top-usernames-shortlist.txt](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt)
```txt
root
admin
test
guest
info
adm
mysql
user
administrator
oracle
ftp
pi
puppet
ansible
ec2-user
vagrant
azureuser
```

#### Step 4:

Use Hydra to bruteforce a login.

- The `-vV` flag is used to display the login attempts.
- The `-L top-usernames-shortlist.txt` flag is used to specify the username list.
- The `-P /usr/share/wordlists/john.lst` flag is used to specify the password list.
- Next, the host, with `-s <port>` to specify the port number.
- `http-get-form <request>` option says that we are trying to log in using a GET request.<br>
  The `<request>` syntax is as follows: `<path>:<form_fields>:<success_or_failure_condition>`
  - The form is located at the root of the website. So we use `/` as path.
  - The form has 4 fields: `page`, `username`, `password` and `Login`.
    - `page` is always `signin`.
    - `username` is the username we are trying. So we use `^USER^`.
    - `password` is the password we are trying. So we use `^PASS^`.
    - `Login` is always `Login`.
  - If the image `WrongAnswer.gif` is displayed, it's a fail.<br>
    So we use `F=<img src=\"images/WrongAnswer.gif\" alt=\"\">`.<br>
    - The `F=` is used to specify the failure condition.
    - And it is followed by the HTML code of the image `WrongAnswer.gif`.

- The full command is:
```bash
hydra -vV -L top-usernames-shortlist.txt -P /usr/share/wordlists/john.lst 192.168.192.18 -s 4243 http-get-form "/:page=signin&username=^USER^&password=^PASS^&Login=Login:F=<img src=\"images/WrongAnswer.gif\" alt=\"\">"
```


> We get the lines :
> ```
> ...
> [4243][http-get-form] host: 192.168.192.18   login: root   password: shadow
> [4243][http-get-form] host: 192.168.192.18   login: admin  password: shadow
> [4243][http-get-form] host: 192.168.192.18   login: test   password: shadow
> [4243][http-get-form] host: 192.168.192.18   login: guest  password: shadow
> [4243][http-get-form] host: 192.168.192.18   login: info   password: shadow
> ...
> ```
> So we deduce that the password is `shadow`, and that the username doesn't matter.


#### Step 5:

- Log in using the password `shadow` (the username doesn't matter).


---

[[Back to main page](/#darkly)]
