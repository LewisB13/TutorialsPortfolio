# Installing Docker Desktop on Windows 11 🐳

## Overview

Docker allows you to run applications inside containers. Containers make it easy to deploy software without worrying about installing lots of dependencies manually.

In this tutorial, we'll install Docker Desktop on Windows 11 and verify that it is working correctly.

---

## Prerequisites

Before starting, ensure you have:

- Windows 11
- Administrator privileges
- An internet connection

---

## Step 1 — Download Docker Desktop

Visit the Docker website:

https://www.docker.com/products/docker-desktop/

Click:

**Download Docker Desktop**

Wait for the installer to finish downloading.

---

## Step 2 — Run the Installer

Locate the downloaded installer and double-click it.

Example:

```text
Docker Desktop Installer.exe
```

Click:

```text
Yes
```

if Windows asks for administrator permissions.

---

## Step 3 — Install Docker Desktop

Follow the installation wizard.

Leave the default settings enabled.

Click:

```text
OK
```

then:

```text
Install
```

The installation may take several minutes.

---

## Step 4 — Restart Your Computer

Once installation is complete, restart Windows if prompted.

---

## Step 5 — Launch Docker Desktop

Open:

```text
Start Menu → Docker Desktop
```

Docker may take a minute to start for the first time.

Accept the licence agreement if prompted.

---

## Step 6 — Verify Docker Is Installed

Open:

```text
Command Prompt
```

or

```text
PowerShell
```

Run:

```powershell
docker --version
```

Example output:

```text
Docker version 28.x.x
```

If you see a version number, Docker has been installed successfully.

---

## Step 7 — Run Your First Container

Run:

```powershell
docker run hello-world
```

Docker will download a small test container.

Example output:

```text
Hello from Docker!
```

If you see this message, Docker is working correctly.

---

## Useful Docker Commands

View running containers:

```powershell
docker ps
```

View all containers:

```powershell
docker ps -a
```

View downloaded images:

```powershell
docker images
```

Stop a container:

```powershell
docker stop CONTAINER_ID
```

Remove a container:

```powershell
docker rm CONTAINER_ID
```

---

## Conclusion

You have successfully:

- Downloaded Docker Desktop
- Installed Docker on Windows 11
- Verified the installation
- Run your first container

Docker is now ready to host applications such as:

- Portainer
- Jellyfin
- Syncthing
- Pi-hole
- Nextcloud

and many other self-hosted services.