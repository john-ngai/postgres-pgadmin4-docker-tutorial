# Tutorial: PostgreSQL, pgAdmin4, and Docker

**Reference:** [marvinjungre. (30 Jun 2023). Setting up PostgreSQL and pgAdmin 4 with Docker. Medium.](https://medium.com/@marvinjungre/get-postgresql-and-pgadmin-4-up-and-running-with-docker-4a8d81048aea)

## Environment Requirements

- Podman Desktop
- pgAdmin4

## Steps

1. Open the Podman Desktop application.

2. Download the latest postgres image:

```sh
podman pull postgres
```

3. Create and run the postgres container:

```sh
podman run --name postgres-db -e POSTGRES_PASSWORD=password -p 5432:5432 -d postgres
```

4. Download the latest pgAdmin 4 image:

```sh
podman pull dpage/pgadmin4
```

5. Create and run the pgAdmin 4 container:

```sh
podman run --name pgadmin-container -p 5050:80 -e PGADMIN_DEFAULT_EMAIL=user@domain.com -e PGADMIN_DEFAULT_PASSWORD=password -d dpage/pgadmin4
```

6. Open pgAdmin via http://localhost:5050/ and login using the previously defined credentials.

7. Use the following command to obtain the postgres container's IP address:

```sh
podman inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' postgres-db
```

8. From pgAdmin, register the new postgres server:

```
Name: postgres-db
Host name/address: <IP addressed obtained from step 7>
Port: 5432
Maintenance database: postgres
Username: postgres
Password: password
```

### Optional Steps for Testing the Connection

1. From pgAdmin, navigate to the `postgres-db` server. Right-click, Create > Database...

```
Database: catbase
```

2. catbase > Schemas > Tables > Create > Table...

#### General Tab

```
Name: cattable
```

#### Columns Tab

```
Name: id
Data type: serial
Not NULL?: true
Primary key?: true
```

```
Name: catname
Data type: text
Not NULL?: true
Primary key?: false
```

3. cattable > View/Edit Data > All Rows

4. From the Data Output tab, click the _Add row_ icon button. Then double-click the cell under the catname column to edit it. Set the value to "Bam Bam" without quotes. Lastly, click the _Save Data Changes_ icon button.

5. Connect to the postgres server via the command line:

```sh
podman exec -it postgres-db psql -U postgres
```

6. Connect to the "catbase" database:

```sh
\c catbase
```

7. Output all of the rows from the cattable:

```sh
SELECT * FROM CATTABLE;
```
