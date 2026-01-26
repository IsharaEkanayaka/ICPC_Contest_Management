# Importing Team Affiliations

This guide describes how to import team affiliations (organizations) into DOMjudge using the command line interface within the Docker container.

## 1. Prepare Data

Ensure your `data/organizations.json` file is formatted correctly.
**Note:** DOMjudge requires ISO 3166-1 alpha-3 (3-letter) country codes (e.g., `LKA`, `BGD`, `IND`).

Example `organizations.json`:

```json
[
    {
        "id": "2",
        "name": "UOP",
        "formal_name": "University of Peradeniya",
        "country": "LKA"
    },
    {
        "id": "4",
        "name": "EWU",
        "formal_name": "East West University",
        "country": "BGD"
    }
]
```

## 2. Copy Data to Container

Copy the JSON file into the `webapp` directory of the `domserver` container.

```bash
docker cp "data/organizations.json" domserver:/opt/domjudge/domserver/webapp/organizations.json
```

## 3. Run Import Command

Execute the import command inside the container using the DOMjudge console.

```bash
docker exec -w /opt/domjudge/domserver/webapp domserver sh -c "bin/console api:call -m POST -f json=organizations.json users/organizations"
```

If successful, you should see output indicating the number of new organizations added:
> "10 new organization(s) successfully added."