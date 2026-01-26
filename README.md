# 🏆 ICPC Contest Management System

This repository contains a complete setup for managing an ICPC-style programming contest. It includes the DOMjudge contest control system, the Contest Data System (CDS) for managing contest data, and various tools for presentations and reporting. ✨

## 📁 Directory Structure

-   `balloonUtil-2.6.1331/`: 🎈 Utility for printing balloon notifications for solved problems.
-   `cds/`: 📊 The Contest Data System (CDS), which acts as a central hub for contest data.
-   `domjudge/`: 💻 The DOMjudge contest control system, including the domserver and database.
-   `judgehosts/`: 🧑‍💻 Configuration for connecting judgehosts to the DOMjudge system.
-   `presentationAdmin-2.6.1331/`: 🖥️ Administration tool for the presentation system.
-   `presentations-2.6.1331/`: 📺 Client for displaying contest presentations (e.g., scoreboards).
-   `resolver-2.6.1331/`: 📈 Tool for generating contest results and awards.

## 🚀 Quick Start

1.  **Run DOMjudge:**
    -   Navigate to the `domjudge/` directory.
    -   Run `docker-compose up` to start the DOMjudge server and database.
    -   The DOMjudge interface will be available at `http://localhost:12345`.

2.  **Run the Contest Data System (CDS):**
    -   Navigate to the `cds/` directory.
    -   **Important:** Before running, update `cdsConfig.xml` with the correct contest number and `accounts.yaml` with your desired credentials (you can use `accounts.yaml.template` as a guide).
    -   Run the CDS with the following command:
        ```powershell
        docker run --name cds --rm -it ^
        -p 8080:8080 -p 8443:8443 ^
        -v %cd%\cdsConfig.xml:/opt/wlp/usr/servers/cds/config/cdsConfig.xml ^
        -v %cd%\accounts.yaml:/opt/wlp/usr/servers/cds/config/accounts.yaml ^
        -v %cd%\data:/data ^
        ghcr.io/icpctools/cds:2.7.1346
        ```

3.  **Set Up Judgehosts:**
    -   Follow the detailed guide in the `judgehosts/` directory to set up your judgehosts.
    -   [**Judgehost Setup Guide**](./judgehosts/GUIDE.md)

## 🛠️ Component Usage

-   **Presentation Admin:**
    -   Navigate to the `presentationAdmin-2.6.1331/` directory.
    -   Run `presAdmin.bat` or `presAdmin.sh` with the following command:
        ```
        presAdmin.bat https://<domjudge-server-ip>:8443 admin admin
        ```

-   **Presentation Client:**
    -   Navigate to the `presentations-2.6.1331/` directory.
    -   Run `client.bat` or `client.sh` with the following command:
        ```
        client.bat https://<cds-server-ip>:8443/api/contests/4 presentation presentation --name "scoreboard"
        ```

-   **Balloon Utility:**
    -   Navigate to the `balloonUtil-2.6.1331/` directory.
    -   Run `balloon.bat` or `balloon.sh`.
    -   In the utility's UI, connect using this information:
        -   **URL:** `https://<cds-server-ip>:8443/api/contests/4`
        -   **User:** `baloon`
        -   **Password:** `baloon`

-   **Resolver:**
    -   Navigate to the `resolver-2.6.1331/` directory.
    -   Run `resolver.bat` or `resolver.sh` with the following command:
        ```
        resolver.bat https://<cds-server-ip>:8443/api/contests/4 admin admin
        ```

## 💾 Backup and Restore

Guides for backing up and restoring the DOMjudge database are located in the `domjudge/` directory:
-   `BACKUP_GUIDE.md`
-   `RESTORE_GUIDE.md`