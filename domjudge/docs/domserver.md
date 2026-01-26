# DOMjudge Docker Management Guide

Here is a list of the most important commands for managing your DOMjudge server. Run these in the directory containing your `docker-compose.yml`.

## 1. Starting & Stopping

### Start everything in the background
Standard way to run the server.
```powershell
docker-compose up -d
```

### Stop and remove containers
Stops containers and removes the network, but **keeps persistent volume data safe**.
```powershell
docker-compose down
```

### Stop containers temporarily
Stops execution but saves container state (faster restart).
```powershell
docker-compose stop
```

### Restart Services
Quickly restart running services (useful after configuration changes).
```powershell
docker-compose restart
```

### Start after PC Reboot
Unless configured otherwise, containers won't start automatically. Run this again after a reboot to restore the server:
```powershell
docker-compose up -d
```

## 2. Monitoring & Logs

### Check status
See if services are "Up" or "Exited", and view port mappings.
```powershell
docker-compose ps
```

### View real-time logs
Follows the logs of all services. Press `Ctrl+C` to exit.
```powershell
docker-compose logs -f
```

### View specific service logs
Only view logs for the DOMserver.
```powershell
docker-compose logs -f domserver
```

## 3. Debugging & Maintenance

### Access DOMserver shell
Opens a bash terminal inside the running DOMserver container.
```powershell
docker-compose exec domserver bash
```

### Access Database shell
Opens a bash terminal inside the running MariaDB container.
```powershell
docker-compose exec mariadb bash
```

### Retrieve Initial Admin Password
Get the default admin password generated on first launch.
```powershell
docker-compose exec domserver cat /opt/domjudge/domserver/etc/initial_admin_password.secret
```

## 4. Data Management

### Backup Database
Dumps the current database state to a `backup.sql` file on your host machine.
*(Note: `-pdjpw` has no space between `-p` and the password)*
```powershell
docker-compose exec mariadb mysqldump -u domjudge -pdjpw domjudge > backup.sql
```

### ⚠️ Destroy Everything (Reset)
**WARNING:** This deletes containers, networks, AND the `mariadb-data` volume. All contest data will be lost.
```powershell
docker-compose down -v
```
