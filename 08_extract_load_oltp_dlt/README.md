# Extract and load from SQL database to snowflake with dlt

Video on dlt theory to extract and load from OLTP database to snowflake :point_down:

[![dlt to extract and load from oltp to snowflake](https://github.com/kokchun/assets/blob/main/data_warehouse/dlt_intro_video.png?raw=true)](https://youtu.be/gfzlxJHfFtA)

## Setup

We will setup postgres and copy in some data, and then we'll setup dlt to load the data into snowflake.

### Postgres

Choose the newest version of postgressql

- [postgressql download](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

Add your password for superuser postgres

Uncheck the button `Launch Stack Builder at exit` as we don't need that tool.

Now go into psql shell and by launching the app and connect to postgres through

```bash
server: localhost
port: 5432
user: postgres
password: <your_password>
```

We will create a user called amazon

```sql
CREATE USER amazon WITH PASSWORD 'your_password';
```

Then we will create a database called amazon.

```sql
CREATE DATABASE amazon;
```

We will now run a .sql file from psql. First we need to connect to

```bash
\c amazon
```

This takes your sql file as input and runs the commands inside the file.

```bash
\i /path/to/your/file.sql
```

> [!TIP]
> To find your path use the bash command `realpath`, e.g. `realpath permissions.sql` or use `readlink -f permissions.sql` if realpath doesn't exist in your system. In git bash in windows you can use `cygpath -m $(pwd)/example.txt`.

### dlt

Start by installing sqlalchemy and psycopg2

```bash
uv pip install sqlalchemy psycopg2-binary
```

Then in `secrets.toml` in .dbt, we add the following

```toml
# put your secret values and credentials here. do not share this file and do not push it to github

[sources.sql_database.credentials]
drivername = "postgresql+psycopg2" # please set me up!
database = "amazon" # please set me up!
password = "amazon_password123" # please set me up!
username = "amazon" # please set me up!
host = "localhost" # please set me up!
port = 5432 # please set me up!

[destination.snowflake.credentials]
database = "amazon" # please set me up!
password = "extract_loader_password123" # please set me up!
username = "extract_loader" # please set me up!
host = "bw01696.west-europe.azure" # please set me up!
warehouse = "dev_wh" # please set me up!
role = "amazon_dlt_role" # please set me up!
```

> [!NOTE]
> Remember to setup snowflake database and roles

> [!NOTE]
> In the python script later, we create the connection string to postgres with the secrets. 



## Other videos :video_camera:




## Read more :eyeglasses:

- [Loading SQL database data to Snowflake using dlt library - dlthub docs](https://dlthub.com/docs/pipelines/sql_database/load-data-with-python-from-sql_database-to-snowflake)