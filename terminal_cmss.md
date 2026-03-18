# Comprehensive SysAdmin Cheat Sheet: Linux, Azure, Docker, & Kubernetes

## System Information & Hardware

| Command | Description |
| :--- | :--- |
| `uname -a` | Displays detailed system information (kernel version, architecture, hostname). |
| `hostname` | Shows or sets the system's hostname. |
| `uptime` | Shows how long the system has been running, along with the load average. |
| `lscpu` | Displays details about the CPU architecture (cores, threads, vendor). |
| `lsblk` | Lists information about all available block devices (hard drives, partitions, flash drives). |
| `free -h` | Displays the total, used, and free memory (RAM and swap) in a human-readable format. |
| `df -h` | Shows disk space usage for all mounted file systems in a human-readable format. |
| `du -sh <dir>` | Calculates and displays the total disk space used by a specific directory. |

---

## File & Directory Management

| Command | Description |
| :--- | :--- |
| `pwd` | Print Working Directory; outputs the absolute path of your current location. |
| `ls -la` | Lists all files (including hidden ones starting with `.`) with detailed permissions, ownership, and size. |
| `cd <dir>` | Changes the current directory to the specified path. `cd ~` goes to your home directory; `cd -` goes to the previous directory. |
| `mkdir -p <dir>` | Creates a new directory. The `-p` flag creates parent directories as needed without throwing an error if they exist. |
| `rm -rf <dir>` | **Use with caution:** Recursively and forcefully removes a directory and all its contents without asking for confirmation. |
| `cp -r <src> <dest>` | Copies files or directories. The `-r` flag is required to copy directories recursively. |
| `mv <src> <dest>` | Moves or renames files and directories. |
| `touch <file>` | Creates an empty file or updates the timestamp of an existing file. |
| `find <dir> -name "<name>"`| Searches recursively for files matching the specified name within the given directory. |

---

## Process Management

| Command | Description |
| :--- | :--- |
| `top` | Displays real-time, dynamic information about running processes and system resource usage. |
| `htop` | An interactive and visually improved alternative to `top` (may require installation). |
| `ps aux` | Displays a snapshot of all currently running processes across all users. |
| `kill <PID>` | Sends a termination signal (SIGTERM) to a specific Process ID to stop it gracefully. |
| `kill -9 <PID>` | **Use with caution:** Sends a SIGKILL signal to forcefully and immediately stop a process. |
| `killall <name>` | Kills all processes matching the specified name. |
| `bg` / `fg` | Sends a suspended process to the background (`bg`) or brings a background process to the foreground (`fg`). |

---

## Users & Permissions

| Command | Description |
| :--- | :--- |
| `su <user>` | Switches the current session to another user. |
| `sudo <command>` | Executes a single command with root (superuser) privileges. |
| `passwd <user>` | Changes the password for the specified user (requires root if changing another user's password). |
| `useradd -m <user>` | Creates a new user account. The `-m` flag creates the user's home directory. |
| `usermod -aG <group> <user>`| Adds a user to an existing group (e.g., the `sudo` or `wheel` group). |
| `chmod 755 <file>` | Changes the read, write, and execute permissions of a file or directory (e.g., 755 = owner rwx, group rx, others rx). |
| `chown <user>:<group> <file>`| Changes the user and group ownership of a file or directory. |

---

## Networking

| Command | Description |
| :--- | :--- |
| `ip a` | Displays all network interfaces and their associated IP addresses (modern replacement for `ifconfig`). |
| `ping <host>` | Sends ICMP echo requests to a network host to test connectivity. |
| `ss -tulpn` | Displays all listening TCP/UDP ports and the programs using them (modern replacement for `netstat`). |
| `curl -O <URL>` | Downloads a file from the internet via command line. |
| `wget <URL>` | Another popular tool for downloading files from the web. |
| `dig <domain>` | Performs DNS lookups and displays information about domain names and their IP addresses. |
| `ssh <user>@<host>` | Connects securely to a remote machine via the Secure Shell protocol. |

---

## System Services (Systemd)

| Command | Description |
| :--- | :--- |
| `systemctl status <svc>` | Shows the current status (running, stopped, failed) of a service. |
| `systemctl start <svc>` | Starts a service immediately. |
| `systemctl stop <svc>` | Stops a running service. |
| `systemctl restart <svc>` | Stops and then immediately starts a service. |
| `systemctl enable <svc>` | Configures a service to start automatically when the system boots. |
| `journalctl -u <svc>` | Displays the system logs specifically for a given service. |

---

## Text Manipulation & Viewing

| Command | Description |
| :--- | :--- |
| `cat <file>` | Outputs the entire content of a file to the terminal. |
| `less <file>` | Allows you to scroll through the contents of a large file one page at a time. |
| `tail -f <file>` | Outputs the last 10 lines of a file and actively "follows" it, displaying new lines as they are added (great for logs). |
| `grep "<text>" <file>` | Searches for a specific text pattern within a file and outputs the matching lines. |
| `awk '{print $1}' <file>` | A powerful text processing tool; this specific example prints the first column of every line in a file. |
| `sed 's/old/new/g' <file>`| A stream editor used here to search for "old" and replace it with "new" globally across the file. |

---

## Archiving & Compression

| Command | Description |
| :--- | :--- |
| `tar -czvf <archive.tar.gz> <dir>`| **C**reates a **z**ipped (gzip) archive, **v**erbosely listing files, to a **f**ile from the specified directory. |
| `tar -xzvf <archive.tar.gz>`| e**X**tracts a **z**ipped archive, **v**erbosely, from a **f**ile into the current directory. |
| `zip -r <archive.zip> <dir>` | Creates a `.zip` file of a directory recursively. |
| `unzip <archive.zip>` | Extracts a `.zip` file into the current directory. |

---

## Azure VM Management (Azure CLI)

> *Note: Run `az login` first to authenticate your session before using these commands.*

| Command | Description |
| :--- | :--- |
| `az vm create -n <name> -g <group> --image <image>` | Creates a new Virtual Machine in the specified resource group using the designated OS image (e.g., `Ubuntu2204`). |
| `az vm list -d -o table` | Lists all VMs in your subscription. The `-d` flag includes current power states, and `-o table` formats the output into a readable grid. |
| `az vm show -n <name> -g <group>` | Displays detailed properties, networking, and metadata of a specific VM (outputs in JSON format by default). |
| `az vm start -n <name> -g <group>` | Powers on a stopped virtual machine. |
| `az vm stop -n <name> -g <group>` | Powers off a VM at the OS level. **Note:** The VM retains its allocated hardware, so compute billing continues. |
| `az vm deallocate -n <name> -g <group>` | Powers off a VM and releases its compute resources back to Azure. This stops compute billing for the VM. |
| `az vm restart -n <name> -g <group>` | Gracefully restarts a running virtual machine. |
| `az vm open-port --port <port> -n <name> -g <group>`| Quickly opens a specific port (like 80 for HTTP or 22 for SSH) by updating the Network Security Group (NSG) attached to the VM. |
| `az vm run-command invoke -g <group> -n <name> --command-id RunShellScript --scripts "<cmd>"` | Executes a shell script directly inside the Linux VM via the Azure agent, which is incredibly useful if you lose SSH access. |
| `az vm delete -n <name> -g <group> --yes` | Deletes the virtual machine. The `--yes` flag bypasses the confirmation prompt. *(You may still need to manually delete the associated OS disk and network interfaces afterward).* |

---

## Container Management (Docker)

| Command | Description |
| :--- | :--- |
| `docker run -d -p <host_port>:<container_port> <image>` | Downloads (if not found locally) and starts a container in the background (`-d`), mapping a port from the host to the container. |
| `docker ps` | Lists all currently **running** containers, their IDs, and mapped ports. |
| `docker ps -a` | Lists **all** containers, including those that are stopped or have exited. |
| `docker pull <image>` | Downloads an image from a registry (like Docker Hub) to your local machine without running it. |
| `docker build -t <name>:<tag> .` | Builds a new Docker image using the `Dockerfile` in the current directory (`.`), tagging it with a custom name. |
| `docker exec -it <container_id> /bin/bash` | Opens an interactive terminal session (`-it`) inside a running container, allowing you to run commands inside it. |
| `docker logs -f <container_id>` | Fetches the logs of a container. The `-f` flag follows the log output live. |
| `docker stop <container_id>` | Gracefully stops a running container. |
| `docker rm <container_id>` | Deletes a stopped container. Use `-f` to forcefully remove a running container. |
| `docker rmi <image_id>` | Removes a locally stored Docker image to free up disk space. |

---

## Container Orchestration (Kubernetes / kubectl)

| Command | Description |
| :--- | :--- |
| `kubectl get nodes` | Lists all the physical or virtual machines (nodes) that make up your Kubernetes cluster. |
| `kubectl get pods -A` | Lists all running Pods (the smallest deployable units in K8s) across all namespaces. |
| `kubectl get svc` | Lists all Services, which expose your Pods to network traffic (shows internal/external IP addresses). |
| `kubectl apply -f <file.yaml>` | The standard way to create or update resources. It reads a YAML manifest file and applies the configuration to the cluster. |
| `kubectl delete -f <file.yaml>` | Deletes the resources defined in the specified YAML file. |
| `kubectl describe pod <pod_name>` | Provides highly detailed information about a specific Pod, including its events and state changes (great for troubleshooting). |
| `kubectl logs <pod_name>` | Prints the standard output/error logs from the container running inside the specified Pod. |
| `kubectl exec -it <pod_name> -- /bin/sh` | Drops you into an interactive shell inside a running Pod, similar to `docker exec`. |
| `kubectl port-forward <pod_name> <local_port>:<pod_port>` | Forwards traffic from your local machine to a Pod. Excellent for testing an app without exposing it publicly. |
| `kubectl scale deployment <name> --replicas=<num>` | Instantly scales a deployment up or down to the specified number of Pod replicas. |
