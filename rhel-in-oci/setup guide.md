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
- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)
- [OCI BYOI Support Policies](https://docs.oracle.com/en-us/iaas/Content/Compute/References/bringyourownimage.htm#options__linux)

---

# Red Hat Subscription Overview
A **Red Hat subscription** provides:

- Access to **Red Hat-tested and certified software packages**.
- Regular **patches, updates, and upgrades** for **RHEL**.
- **Technical support** and a **knowledge base**.

## **Prerequisites**
Ensure you have an **active Red Hat subscription** to Download **Red Hat Enterprise Linux**.

If you try to install any software on the system without registering the system with Red Hat, you'll see the following error messgae:
<img width="800" alt="image" src="https://github.com/user-attachments/assets/d637639c-23ea-453c-9701-58963590003e" />

  


---

# Register and Enable Red Hat Subscription

## **1. Registering via Web Console**
- [Log in](https://www.redhat.com/wapps/ugc/register.html?_flowId=register-flow&_flowExecutionKey=e1s1) to **RHEL Web Console**.
- Click on the **Avatar (top-right)** → **Subscriptions** → **Overview**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/f1e2d963-66ce-4e82-ba93-d281d1d5c048" />

---

## **2. Registering via CLI**
- SSH into the RHEL instance. Reset the root password.
  ```bash
  sudo passwd root
  ```
  <img width="800" alt="image" src="https://github.com/user-attachments/assets/27f3156d-80cb-4bf4-80dd-58df495dc76d" />
- Run the following command:
```bash
sudo subscription-manager register
```
   This registers the RHEL instance. Authenticate with the root password. Use the Red Hat username and password to register the instance to your account.
   <img width="800" alt="image" src="https://github.com/user-attachments/assets/a527c8cc-4b28-48c5-b128-0ae7e53ab3a5" />

   The server is registered with the account and it is reflected in your Red Hat Account
   <img width="800" alt="image" src="https://github.com/user-attachments/assets/481f2cbc-0f25-44ef-a1b5-45ab416fbd7b" />


## 3. Attach Subscription
- In the **Red Hat Portal**, click on the **Host Name** of the server.
- Click **Attach Subscription**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/0b8eaa49-3ac0-4644-92ca-6e6cdcdcca76" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3ee25865-c664-4ad6-a8ec-7c228e78c0cf" />


---

## 4. Verify Subscription
Run the following command to check the attached subscription:
```bash
sudo subscription-manager list --consumed
```
<img width="800" alt="image" src="https://github.com/user-attachments/assets/14243231-3b75-45b4-a9b0-9de3dbb421f8" />

## 5. Update the System
To update all system packages, run:
```bash
sudo yum update
```
## Attaching a Block Volume

### 1. Create a Block Volume

1. In the OCI Console, go to **Storage** → **Block Volume** → **Create Block Volume**.
2. Configure the size and performance settings.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/86a3d6b2-0748-41ae-a3eb-cb0f0611fac0" />





### 2. Attach Block Volume to Instance

1. Navigate to **Compute** → **Instances**.
2. Select the instance → **Attached Block Volumes** → **Attach Block Volume**.
3. Choose the volume and click **Attach**.
   <img width="800" alt="image" src="https://github.com/user-attachments/assets/acb76ec4-e177-49f3-9ac8-1c9c7097e826" />

### 3. Connect to the Block Volume

If using iSCSI attachment, follow these steps:

#### 1. Get iSCSI Commands
1. In the OCI Console, select the Block Volume.

2. Click **Actions (⋮) → iSCSI Commands and Information**.

3. Copy and execute the iSCSI commands in the instance.
<img width="930" alt="image" src="https://github.com/user-attachments/assets/f3fc63e3-8a70-4a4a-8afb-d56cfe4fd011" />

#### 2. Install iSCSI Tools

1. Log in (SSH) to the server. Before the iSCSI commands, install the iSCSI package
2. Copy the commands and paste them into your instance session window for each of the following steps

```bash
sudo yum install iscsi-initiator-utils
```
