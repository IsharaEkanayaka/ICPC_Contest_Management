# Importing Teams

Teams were imported into the DOMjudge server using the JSON format derived from `data/team_details.csv` and mapped to organizations in `data/organizations.json`.

## Data Source
- **Source File**: `data/teams.json`
- **Format**: JSON [DOMjudge Teams Import Documentation](https://www.domjudge.org/docs/manual/main/import.html#id1)

## Import Process

The import was performed via the command line of the `domserver` Docker container.

### 1. Copy Data to Container
Copy the prepared `teams.json` file into the running `domserver` container.

```powershell
docker cp "data/teams.json" domserver:/tmp/teams.json
```

### 2. Run Import Command
Execute the DOMjudge console command inside the container to import the teams via the internal API.

```powershell
docker exec domserver /opt/domjudge/domserver/webapp/bin/console api:call -m POST -f json=/tmp/teams.json users/teams
```

## Success Output
The command returns a confirmation of the number of teams added:

```
"22 new team(s) successfully added."
```
