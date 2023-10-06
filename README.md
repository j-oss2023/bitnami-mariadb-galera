# MariaDB Galera Multi-Master Cluster Provisioning

Set up a MariaDB Galera Multi-Master cluster effortlessly using Docker. Find below an informative guideline to facilitate a successful deployment.

## 🧰 Prerequisites

- **Docker and Docker Compose** installed on your machine.
- Familiarity with **MariaDB**, **Galera**, and **Docker**.

## 🛠️ Configuration

Utilize the following environment variables for database and user setup:

- `MARIADB_DATABASE`: To specify your database name.
- `MARIADB_USER`: To create a restricted user for the specified database.
- `MARIADB_PASSWORD`: To define the user's password.

🔐 **Security**: Ensure to substitute root, backup, and replication passwords with robust and secure alternatives. Ensure consistency across all nodes.

## 🚀 Deployment

### Persistent Mounts

Ensure persistent mounts are owned by UID 1001, since `bitnami/mariadb-galera` runs with this UID:

```bash
mkdir ./node1; chown 1001:1001 ./node1
mkdir ./node2; chown 1001:1001 ./node2
```

### Cluster Setup

#### Step 1: Initialize Node 1

Start the primary node of your cluster independently:

```bash
docker compose up node1 -d
```

🕵️ **Verify**: Monitor logs and look for status indicators like:

```plaintext
<<<<<<< HEAD
Synchronized with group, ready for connections
=======
WSREP: wsrep_notify_cmd is not defined, skipping notification.
>>>>>>> 82d6e47c87842ef21eac0c1aa09aedd0ca187a97
```

#### Step 2: Launch Node 2

With node1 stable, initiate node2:

```bash
docker compose up node2 -d
```

Ensure to examine the logs for consistent cluster formation messages as with node1.

## 🛑 Shutdown and Maintenance

🚨 **Critical**: If using `docker compose down`, ensure to adjust `grastate.dat` on **node1** and **node2**: - in order to be able to boot the cluster again.

```plaintext
safe_to_bootstrap: 1
```

## 📝 Additional Guidelines

- Ensure network and storage sufficiency for all nodes.
- Replicate the node setup steps for larger deployments.
- Monitor nodes regularly, especially during and after scaling, to mitigate synchronization issues.

## 🔍 Troubleshooting

- Confirm accurate setting of environment variables in the Docker Compose file.
- Check logs for errors or failed synchronization attempts between nodes.
- Validate the MariaDB Galera cluster's status, examining cluster size and membership.

## 👥 Contribution

Feel free to contribute by submitting pull requests or creating issues for enhancements and bug reports.

---

**💡 Enhancement Tips:**

- Develop a shell script to automate node setup and `grastate.dat` adjustments.
- Implement a monitoring mechanism for continual node health verification.
- Adopt Docker secrets or another secure methodology for managing secrets.
- Provide examples of troubleshooting scenarios and their solutions.
```
