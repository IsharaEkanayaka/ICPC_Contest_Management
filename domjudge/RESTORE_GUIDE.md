# Restoring DOMjudge Setup on a New PC

Follow these steps to restore your DOMjudge contest environment on another computer:

## 1. Prerequisites
- Install Docker and Docker Compose on the new PC.
- Copy the following files to a folder (e.g., `D:\domjudge`):
  - `docker-compose.yml`
  - `mariadb-data-backup.tar.gz`
  - `busybox.tar` (if the new PC is offline)
  - `domjudge_icpc_sl_2026.sql` (if you need the SQL dump)

## 2. Load the BusyBox Image (if offline)
If the new PC does not have internet access, load the BusyBox image:

```powershell
docker load -i busybox.tar
```

## 3. Create the MariaDB Data Volume

```powershell
docker volume create domjudge_mariadb-data
```

## 4. Restore the Database Volume

Replace `D:\domjudge` with the path where your backup is located:

```powershell
docker run --rm -v domjudge_mariadb-data:/volume -v D:\domjudge:/backup busybox tar xzf /backup/mariadb-data-backup.tar.gz -C /volume
```

## 5. Start DOMjudge

```powershell
docker-compose up
```

## 6. (Optional) Restore from SQL Dump
If you want to restore from the SQL dump instead of the volume backup, use:

```powershell
docker cp domjudge_icpc_sl_2026.sql domserver:/domjudge_icpc_sl_2026.sql
docker exec -it domserver bash
mysql -u domjudge -p -h mariadb -P 3306 domjudge < /domjudge_icpc_sl_2026.sql
```

---

**Your DOMjudge environment should now be restored and ready to use!**
