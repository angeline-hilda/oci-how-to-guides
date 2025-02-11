# Guide to DBCS Auto Scaling in Oracle Cloud Infrastructure (OCI)

## Introduction
Oracle Database Cloud Service (DBCS) provides a fully managed environment where tasks such as backups, patching, and database parameter configurations are handled seamlessly. To optimize costs, the OCPU configuration of DBCS can be adjusted based on business hours. This guide demonstrates how to automate scaling up and down of a DBCS instance using OCI CLI and cron jobs.

---

## Prerequisites for DBCS Auto Scaling
Before you begin, ensure the following:

1. **Bastion Host**:  
   - A Linux compute instance that will run the scaling scripts.  
   - It can be a free-tier instance or one with minimal OCPU and memory configuration to optimize costs.  
   - This instance must run 24/7 to execute the scaling scripts.

2. **OCI CLI**:  
   - Installed on the Bastion host.  
   - Used to execute commands for scaling the DBCS instance.

3. **User Credentials**:  
   - OCI CLI must be configured with user credentials that have permissions to update the DBCS system.
4. **DBCS system**
   - A DBCS ssystem with **two nodes**, each initially configured with **4 OCPUs** (total **8 OCPUs**) to perform the sample scaling operations.


---

## Step 1: Create a Bastion Host
A Bastion host is a compute instance used to execute the scaling scripts. Follow these steps to set it up:

1. In the OCI Console, navigate to **Compute** → **Instances** → **Create Instance**.
2. Choose a Linux-based operating system (e.g., Oracle Linux or Ubuntu).
3. Select a minimal OCPU and memory configuration (e.g., 1 OCPU and 1 GB RAM) to minimize costs.
4. Configure networking and SSH keys for secure access.
5. Launch the instance and ensure it is running 24/7.

---

## Step 2: Install OCI Command Line Interface (CLI)
The OCI CLI is required to execute commands for scaling the DBCS instance. Follow these steps to install and configure it:

1. **Install OCI CLI**:  
   Run the following commands on the Bastion host:  
   ```bash
   bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"

2. **Configure OCI CLI**:
Run the following command to configure the CLI with your credentials:

```bash
oci setup config
```

Provide the required details:
- OCI User OCID
- Tenancy OCID
- Region
- API Key

3. **Verify Installation**:
Test the CLI by running:

```bash
oci iam availability-domain list
```

## Step 2: Create Scaling Scripts

Right now a DBCS instance has 8 cores (2 node each with 4 OCPU), we are scaling down it by 6 cores (2 node each with 3 OCPU), which can be done with the below command.

### Scale Down Script (`/root/scaledown.sh`)

```bash
#!/bin/bash
/root/bin/oci db system update --db-system-id <ocid1.dbsystem......> --cpu-core-count 6
```
### Scale Up Script (`/root/scaleup.sh`)

```bash
#!/bin/bash
/root/bin/oci db system update --db-system-id <ocid1.dbsystem......> --cpu-core-count 8
```

Replace `<ocid1.dbsystem......>` with the OCID of your DBCS instance.

### Make Scripts Executable

```bash
chmod +x /root/scaleup.sh
chmod +x /root/scaledown.sh
```

## Step 3: Schedule Scaling with Cron Jobs

### Edit the Crontab File

```bash
crontab -e
```

### Add Cron Entries

```bash
# Scale up at 4:30 AM every day
30 4 * * * /root/scaleup.sh

# Scale down at 2:30 PM every day
30 14 * * * /root/scaledown.sh
```

### Verify Cron Jobs

```bash
crontab -l
```

## Step 4: Monitor and Verify Scaling

### Check DBCS OCPU Count

```bash
oci db system get --db-system-id <ocid1.dbsystem......>
```

### Review Logs

```bash
tail -f /var/log/cron
```

## Conclusion
By following this guide, you can automate the scaling of your DBCS instance in OCI to optimize costs based on business hours. This setup ensures that your database resources are scaled up during peak hours and scaled down during off-peak hours, reducing unnecessary resource consumption.

