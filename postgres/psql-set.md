# Using the --set flag in psql

I had a use case where I had written a script to be passed into the `psql`
command via the `--file` flag, and I wanted to be able to parameterize it, as
it relied on some sensitive information such as role passwords.

After some searching I found out about the `--set` flag.

Given the following script:

```sql
CREATE ROLE admin_user WITH PASSWORD 'mysecurepassword' LOGIN;
```

If you're checking this file into source control you obviously do not want
to store the password in plaintext here, so what can you do?

Turns out that you can pass variable assignments to the `--set` flag supported
by `psql`. So in our case, we could rewrite our script as:

```sql
CREATE ROLE admin_user WITH PASSWORD :'my_password_var' LOGIN;
```

And run the script like so:

`psql --set=my_password_var=mysecurepassword --file=./my_file.psql`

And then we can use our script to provision a database without storing
any sensitive information!
