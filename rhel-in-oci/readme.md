# Launch RHEL Instance in OCI

## 1. Download RHEL Image from Red Hat
To launch a RHEL instance in OCI, you need a supported image format:
- **KVM Guest Image**: Downloaded from the [Red Hat Customer Portal](https://access.redhat.com).
- **QCOW2 Image**: Created using the [Red Hat Image Builder Tool](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/composing_a_customized_rhel_system_image/index).

From the **Red Hat Enterprise Linux Download Page**, download the **KVM Guest Image**.

![Red Hat Customer Portal](image.png)

---

## 2. Upload the RHEL Image to OCI Object Storage
Once downloaded:
1. Log in to the **Oracle Cloud Console**.
2. Navigate to **Object Storage** → **Create a Bucket** (if not already created).
3. Upload the RHEL image to this bucket.

---

## 3. Import the RHEL Image as a Custom Image
After uploading, import the image into OCI:
1. Navigate to **Compute** → **Custom Images**.
2. Click **Import Image** → Select the uploaded image from the bucket.
3. Configure:
   - **Image Format:** `QCOW2`
   - **Launch Mode:** `Paravirtualized`
4. Click **Import Image**.

---

## 4. Create a Compute Instance
1. Navigate to **Compute** → **Instances** → **Create Instance**.
2. Click **Change Image** → **My Images** → Select the imported RHEL image.
3. Choose a **compatible shape**.
4. Configure networking, boot volume, and SSH keys.
5. Click **Create**.

---

## 5. Connect to the Instance
After launch, connect via SSH:
```bash
ssh -i <private-key-path> cloud-user@<instance-public-ip>
```
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
