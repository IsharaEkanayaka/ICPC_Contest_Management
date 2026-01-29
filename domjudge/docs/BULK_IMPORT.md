# 📦 DOMjudge Bulk Data Import Guide

This guide outlines the process for importing contest data (Organizations, Teams, and Accounts) into DOMjudge in bulk.

## ⚠️ Important: Import Order
To avoid errors due to missing dependencies, you **must** import data in the following order:

1.  **[Organizations (Affiliations)](./importing%20data/team_affiliations.md)**
    *   *Why:* Teams belong to organizations. Organizations must exist first.
2.  **[Teams](./importing%20data/teams.md)**
    *   *Why:* Teams link to organizations. Team Accounts link to teams.
3.  **[Accounts](./importing%20data/accounts.md)**
    *   *Why:* specific user accounts (like team logins) are attached to existing teams.

## 📄 Data Files
You should prepare your data files in the `domjudge/data/` directory.

-   `organizations.json`: List of universities or groups.
-   `teams.json`: List of competing teams.
-   `accounts.yaml`: User credentials for teams and jury members.

## 🛠️ Import Methods

### Method 1: Web Interface (Easiest)
1.  Log in as **admin**.
2.  Go to the **Jury Interface**.
3.  Navigate to **Import / export** (in the top menu or sidebar).
4.  Under **Import JSON / YAML**, upload your files in the correct order:
    1.  Select `organizations.json` and click Import.
    2.  Select `teams.json` and click Import.
    3.  Select `accounts.yaml` and click Import.

### Method 2: Docker CLI (Automatable)
This method allows you to script the import process. See the detailed guides linked above for the exact commands for each file type.

**General syntax:**
```bash
docker cp data/<file> domserver:/tmp/<file>
docker exec domserver /opt/domjudge/domserver/webapp/bin/console api:call -m POST -f <format>=/tmp/<file> users/<endpoint>
```

## 📚 References
-   [Official DOMjudge Import Manual](https://www.domjudge.org/docs/manual/main/import.html)
