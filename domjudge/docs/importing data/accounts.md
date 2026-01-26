# Importing Accounts to DOMjudge

This document describes how to import accounts from `data/accounts.yaml` into DOMjudge.

## Prerequisites

- DOMjudge server must be running (via docker-compose)
- `data/accounts.yaml` file must be prepared with all team accounts

## Import Methods

### Method 1: Using Docker CLI (Recommended)

This method copies the accounts file to the container and imports it using the DOMjudge console command.

```bash
# Copy the accounts file to the domserver container
docker cp data/accounts.yaml domserver:/tmp/accounts.yaml

# Import the accounts using the DOMjudge console
docker exec domserver /opt/domjudge/domserver/webapp/bin/console api:call -m POST -f yaml=/tmp/accounts.yaml users/accounts
```

**One-liner command:**
```bash
docker cp data/accounts.yaml domserver:/tmp/accounts.yaml; docker exec domserver /opt/domjudge/domserver/webapp/bin/console api:call -m POST -f yaml=/tmp/accounts.yaml users/accounts
```

### Method 2: Using the Web Interface

1. Access DOMjudge at http://localhost
2. Log in with admin credentials
3. Go to **Jury** interface
4. Navigate to **Import / export**
5. Under "Import JSON / YAML", select **accounts**
6. Choose your `data/accounts.yaml` file
7. Click **Import**

### Method 3: Using the API (with HTTPie)

If you have HTTPie installed:

```bash
http --check-status -b -f POST "http://localhost/api/users/accounts" yaml@data/accounts.yaml
```

## Expected Output

When successful, you should see:
```
"22 new account(s) successfully added."
```

## Verification

After importing:
1. Log into the Jury interface
2. Navigate to **Users**
3. Verify all 22 team accounts are listed
4. Test login with one of the team credentials from `data/accounts.yaml`

## Troubleshooting

- If the container path is incorrect, use: `docker exec domserver ls -la /opt/domjudge/domserver/webapp/bin/` to verify
- Ensure the domserver container is running: `docker ps | grep domserver`
- Check container logs if import fails: `docker logs domserver`

## Account Format

Each account in `data/accounts.yaml` follows this structure:
```yaml
- id: icpcslg004
  username: icpcslg004
  password: R7f@K9mQ2
  type: team
  team_id: 1
  name: AfterShock
```

## References

- [DOMjudge Official Documentation - Importing Accounts](https://www.domjudge.org/docs/manual/main/import.html#importing-accounts)
