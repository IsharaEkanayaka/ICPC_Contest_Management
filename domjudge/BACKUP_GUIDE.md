# Backing Up Your DOMjudge Environment

This guide provides two methods to back up your DOMjudge contest data.

## Prerequisites
- Install Docker and Docker Compose.
- Ensure the `busybox` Docker image is available.
  - If you are online, you can pull it:
    ```powershell
    docker pull busybox
    ```
  - If you are offline and have a `busybox.tar` file, load it:
    ```powershell
    docker load -i busybox.tar
    ```

## Method 1: Back Up the Database Volume

This method creates a full backup of the MariaDB data volume.

### 1. Stop DOMjudge

It's recommended to stop the containers before creating a backup to ensure data consistency.

```powershell
docker-compose down
```

### 2. Back Up the Volume

This command creates a compressed archive of the `domjudge_mariadb-data` volume. Replace `D:\domjudge` with the directory where you want to store the backup.

```powershell
docker run --rm -v domjudge_mariadb-data:/volume -v D:\domjudge:/backup busybox tar czf /backup/mariadb-data-backup.tar.gz -C /volume .
```

### 3. Restart DOMjudge

```powershell
docker-compose up -d
```

Your backup file (`mariadb-data-backup.tar.gz`) will be located in the specified backup directory.

## Method 2: Create a SQL Dump

This method creates a `.sql` file containing all the data from the `domjudge` database. This is useful for migrating data or for creating a more portable backup.

### 1. Ensure DOMjudge is Running

The containers must be running to create a SQL dump.

```powershell
docker-compose up -d
```

### 2. Create the SQL Dump

This command executes `mysqldump` inside the `domserver` container to create the SQL backup file.

```powershell
docker exec domserver mysqldump --user=domjudge --password=djpw --host=mariadb domjudge > domjudge_icpc_sl_2026.sql
```

Your SQL dump file (`domjudge_icpc_sl_2026.sql`) will be created in the same directory as your `docker-compose.yml` file.

---

**It is recommended to perform both backup methods for complete data safety.**
