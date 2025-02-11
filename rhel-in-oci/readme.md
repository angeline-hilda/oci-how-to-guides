# Launch RHEL Instance in OCI

## 1. Download RHEL Image from Red Hat
To launch a RHEL instance in OCI, you need an image in one of the supported format:
- **KVM Guest Image**: Downloaded from the [Red Hat Customer Portal](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.9/x86_64/product-software).
- **QCOW2 Image**: Created using the [Red Hat Image Builder Tool](https://www.redhat.com/sysadmin/rhel-image-builder).

From the **Red Hat Enterprise Linux Download Page**, download the **KVM Guest Image**.

<img width="800" alt="redhat_KVM_image" src="https://github.com/user-attachments/assets/791531d8-b3e5-4a32-9335-a01be2a02204" />

---

## 2. Upload the RHEL Image to OCI Object Storage
Once downloaded:
1. Log in to the **Oracle Cloud Console**.
2. Navigate to **Object Storage** → **Create a Bucket** (if not already created).
3. Upload the RHEL image to this bucket.
<img width="800" alt="object_storage" src="https://github.com/user-attachments/assets/04ef3ac2-36d1-44ea-b7f7-f4bd697f2715" />



---

## 3. Import the RHEL Image as a Custom Image
After uploading, import the image into OCI:
1. Navigate to **Compute** → **Custom Images**.
2. Click **Import Image** → Select the uploaded image from the bucket.
3. Configure:
   - **Image Format:** `QCOW2`
   - **Launch Mode:** `Paravirtualized`
4. Click **Import Image**.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/f9103a88-04e7-4ee9-b4bb-068cd177319d" />

<img width="800" alt="custome_image" src="https://github.com/user-attachments/assets/a3e209a8-1121-41fd-8fc7-97e522b98bc1" />



---

## 4. Create a Compute Instance
1. Navigate to **Compute** → **Instances** → **Create Instance**.
2. Click **Change Image** → **My Images** → Select the imported RHEL image.
3. Choose a **compatible shape**.
4. Configure networking, boot volume, and SSH keys.
5. Click **Create**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3f9a4896-5b28-4612-bb47-1b219244f36f" />

<img width="809" alt="image" src="https://github.com/user-attachments/assets/7bdc3a82-82d2-43db-81cf-f732a4c303ed" />



---

## 5. Connect to the Instance
After launch, connect via SSH:
```bash
ssh -i <private-key-path> cloud-user@<instance-public-ip>
```

<img width="803" alt="image" src="https://github.com/user-attachments/assets/a19ff4f9-9e7d-4e0d-ba47-6beefb333255" />

# Licensing and Subscription Details

## BYOS Model (Bring Your Own Subscription)
RHEL images require **registration** with **Red Hat Subscription Manager**.

### **Billing**
- **OCI charges** apply based on instance shape and usage.
- **RHEL license fees** are handled directly via **Red Hat**.

### **Support**
- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates)
- [OCI BYOI Support Policies](https://www.oracle.com/cloud/compute/bring-your-own-image.html)

---

# Red Hat Subscription Overview
A **Red Hat subscription** provides:

- Access to **Red Hat-tested and certified software packages**.
- Regular **patches, updates, and upgrades** for **RHEL**.
- **Technical support** and a **knowledge base**.

## **Prerequisites**
Ensure you have an **active Red Hat subscription** to:

- Download **Red Hat Enterprise Linux**.
- Register and enable your system with **Red Hat**.

---

# Register and Enable Red Hat Subscription

## **1. Registering via Web Console**
- Log in to **RHEL Web Console**.
- Click on the **Avatar (top-right)** → **Subscriptions** → **Overview**.

---

## **2. Registering via CLI**
Run the following command:
```bash
sudo subscription-manager register
```
## 3. Attach Subscription
- In the **Red Hat Portal**, click on the **Host Name** of the server.
- Click **Attach Subscription**.

---

## 4. Verify Subscription
Run the following command to check the attached subscription:
```bash
sudo subscription-manager list --consumed
```
## 5. Update the System
To update all system packages, run:
```bash
sudo yum update
```
## Attaching a Block Volume

### 1. Create a Block Volume

1. In the OCI Console, go to **Storage** → **Block Volume** → **Create Block Volume**.
2. Configure the size and performance settings.

### 2. Attach Block Volume to Instance

1. Navigate to **Compute** → **Instances**.
2. Select the instance → **Attached Block Volumes** → **Attach Block Volume**.
3. Choose the volume and click **Attach**.

### 3. Connect to the Block Volume

If using iSCSI attachment, follow these steps:

#### 1. Install iSCSI Tools

```bash
sudo yum install iscsi-initiator-utils
```
#### 2. Get iSCSI Commands
1. In the OCI Console, select the Block Volume.

2. Click **Actions (⋮) → iSCSI Commands and Information**.

3. Copy and execute the iSCSI commands in the instance.
