# SEA8 - Spring Quest - SQL Injection - Practice with Spring

## Challenge: Fill the holes

This challenge consists of two distinct tasks:

The *required* task is fix the 3 SQL injection vulnerabilities affecting:

1. the credit cards list: **/customers/*identifier*/credit-cards** (GET)
2. the sign-in process: **/customers/authenticate** (POST)
3. the profile update process: **/customers/update** (POST)

The *optional* task: find out how to exploit the vulnerability in the SQL profile update.

```TEXT
### BankZecure Web Application

The fake customer accounts' credentials are listed [here](https://github.com/WildCodeSchool/quest-springboot-sql-injection/blob/master/FakeAccountsCredentials.md).
```

**As for the required task**, you have to replace the **Statement**s and the *dynamic* queries (built by concatenating string elements) with **PreparedStatement**s and parameterized queries (where the query contains **?** placeholders, in which values are injected via **setString**, etc.).

This has to be made in all the methods of **CustomerRepository** and **CreditCardRepository**.

Find inspiration and examples in the JDBC quests (especially *2: Insert* and *3: Update*).

**As for the optional task**: you have to look closely at the profile update form, *and* the update **query** in **CustomerRepository.update**. Here are a few hints:

- There's more to the update form than meets the eye... Which means that the form field you have to tamper with is not necessarily the one you **see**. *Use the inspector!*
- You may find it useful to try the malicious SQL strings that were used to exploit the other vulnerabilities.
- When the outcome of your attack is apparently successful (*Profile updated!*), check what happened in the **customer** table.

If you figure out the 3rd vulnerability, **describe it in your fork's README.md file**.

Push your fixed application to GitHub and share the link of your fork.

## Validation criterias

- **Mandatory: Statement** is not used anymore in CustomerRepository and CreditCardRepository.
- **Mandatory:** parameterized queries replace dynamic queries.
- **Optional:** the **README.md** file contains a description of how you exploited the vulnerability, and what was its outcome. No need to write a novel though: 2 lines should do!

## Optional Task

There is no further validation when calling the CustomerRepository.update method. Anyone who knows the URL with the correct POST method and the required attributes from the web page form (e.g. with Inspect from Chrome) can call it and manipulate data.

Possible scenario:
Someone uses a script that serially goes through all the integer identifiers from 1 to n. Either the error message "Error: account not found or incorrect password" is displayed or the email address and password are reset without any further questions. This would give you all existing identifiers with a newly set uniform password.

e.g. with Postman
POST <http://localhost:8080/customers/update?identifier=125054&email=someone@somewhere&password=unsecret>

mysql> select * from customer where identifier=125054;

|id|identifier|first_name|last_name|email|password|
|--|----------|----------|---------|-----|--------|
|1|125054|Allie|Padberg|someone@somewhere|unsecret|

1 row in set (0.00 sec)
