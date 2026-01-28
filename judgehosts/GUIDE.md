# 🧑‍💻 Setting Up DOMjudge Judgehosts

This guide provides detailed instructions for setting up judgehosts to connect to your DOMjudge contest control system. For a robust setup, we recommend using at least two separate PCs as judgehosts, with each running at least two judgedaemons.

## 🖥️ Recommended Setup

-   **Hardware:** At least 2 physical PCs.
-   **Judgedaemons:** At least 2 per PC.
-   **Operating System:** Ubuntu Server (running inside a VM if your host PC is Windows).

## 🪟 Setting Up on Windows

If you are using Windows on your judgehost machines, you will need to use virtualization to run the judgedaemons in a Linux environment.

### 1. Install VirtualBox and Ubuntu Server

1.  Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) on your Windows PC.
2.  Download the latest [Ubuntu Server LTS](https://ubuntu.com/download/server) ISO image.
3.  Create a new virtual machine (VM) in VirtualBox and install Ubuntu Server using the downloaded ISO.

### 2. Configure VM Networking

To ensure you can connect to the VM from your host machine (e.g., via SSH), you need to configure the VM's network adapter. You have two main options:

-   **NAT with Port Forwarding:** This is the default setting. You can configure port forwarding in the VM's network settings to forward a port from the host to the VM's SSH port (22).
-   **Bridged Adapter:** This will make your VM appear as a separate device on your network, getting its own IP address. This is often simpler for network access.

After configuring networking, start the VM and find its IP address (`ip a`). You can then SSH into the VM from your host machine.

### 3. Enable cgroupv1

DOMjudge requires cgroupv1 to be enabled for resource control. Most modern Linux distributions, including recent Ubuntu versions, use cgroupv2 by default. You must switch back to the legacy cgroupv1.

1.  **Edit the GRUB configuration file:**
    ```bash
    sudo nano /etc/default/grub
    ```

2.  **Modify the `GRUB_CMDLINE_LINUX` line.** Add `systemd.unified_cgroup_hierarchy=0` to the line. It should look something like this:
    ```
    GRUB_CMDLINE_LINUX="systemd.unified_cgroup_hierarchy=0"
    ```

3.  **Update GRUB and reboot:**
    ```bash
    sudo update-grub
    sudo reboot
    ```

After the VM reboots, cgroupv1 will be enabled.

### 4. Install Docker

The judgedaemons will run in Docker containers. Install Docker and Docker Compose inside the Ubuntu VM.

1.  **Install Docker:** Follow the official instructions to [install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/). This will also install the `docker-compose-plugin`.

### 5. Deploy the Judgehosts

1.  **Create a directory for your judgehost configuration:**
    ```bash
    mkdir ~/judgehosts
    cd ~/judgehosts
    ```

2.  **Copy `docker-compose.yml`:** Copy the `docker-compose.yml` file from this directory (`judgehosts/docker-compose.yml` in the main project) into the `~/judgehosts` directory inside your VM.

3.  **Start the judgedaemons:**
    ```bash
    docker compose up -d
    ```

    This will start the judgedaemons, which will then connect to the DOMjudge server. You can check the logs to ensure they are running correctly:
    ```bash
    docker compose logs -f
    ```

### 6. Create Judgehost User in DOMjudge

Before the judgedaemons can connect to the DOMjudge server, you need to create a user for them with the correct permissions.

1.  **Log in to the DOMjudge web interface** as an administrator.
2.  Navigate to the **Users** section.
3.  Click **Add new user**.
4.  Fill in the user details:
    -   **Username:** `judgehost` (or another name, but you'll need to update the `docker-compose.yml` file).
    -   **Password:** Create a strong password. This will be the `JUDGEDAEMON_PASSWORD` in your `docker-compose.yml` file.
5.  **Assign Roles:**
    -   Select the **Jury** role.
    -   Select the **Judgehost** role.
6.  **Save the user.**

## 📄 `docker-compose.yml` Configuration

The provided `docker-compose.yml` file is configured to run two judgedaemons. You will need to edit this file to match your DOMjudge server's configuration.

-   **`DOMSERVER_BASEURL`**: Set this to the URL of your DOMjudge server.
-   **`JUDGEDAEMON_PASSWORD`**: Set this to the password you configured for the judgehost in the DOMjudge web interface.

By following these steps, you will have a robust judgehost setup ready for your contest.
