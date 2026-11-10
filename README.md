# Burp Suite Professional 2026 — Setup & Java Configuration Guide

This repository contains the complete installation instructions, troubleshooting steps, and video walkthrough guides for running **Burp Suite Professional (v2026.x)**.

---

## 1. How to Downgrade & Configure Java Version (Video Walkthrough 1)

Burp Suite Pro and its loader require a specific compatible Java Runtime Environment (JRE/JDK 21). Running incompatible or bleeding-edge Java versions can lead to JVM crash errors or bytecode agent failures.

### Video Recording: Java Downgrade Guide
> **[Watch Recording 1: Java Downgrade Walkthrough](recordings/01-downgrade-java.mp4)**  
> *(Replace the link or filename above with your actual uploaded video file / YouTube link)*

```
[▶ Video 1: Step-by-step Java Downgrade & Alternatives Configuration]
```

---

### Step-by-Step Instructions:

#### Step 1.1: Verify Your Current Java Version
Check what version of Java is currently set as active:
```bash
java -version
```

#### Step 1.2: Install Compatible OpenJDK (Java 21 JRE/JDK)
On Debian / Ubuntu / Kali Linux:
```bash
sudo apt update
sudo apt install -y openjdk-21-jre openjdk-21-jdk
```

#### Step 1.3: Downgrade / Select the Active Java Version
Use `update-alternatives` to switch your active Java runtime to OpenJDK 21:
```bash
sudo update-alternatives --config java
```
You will be prompted with a list of installed Java paths. Enter the selection number corresponding to **`java-21-openjdk`** (e.g. `/usr/lib/jvm/java-21-openjdk-amd64/bin/java`).

*(Optional)* If you compile or use javac tools, switch compiler alternatives as well:
```bash
sudo update-alternatives --config javac
```

#### Step 1.4: Confirm the Downgrade
Verify that the active version has switched:
```bash
java -version
```
*Expected output: OpenJDK Runtime Environment (build 21.x.x...)*

---

## 2. Burp Suite Professional Setup & Execution (Video Walkthrough 2)

Once the compatible Java version is configured, follow the setup procedure below to initialize the loader, generate the license, and start Burp Suite Pro.

### Video Recording: Setup & Activation Guide
> **[Watch Recording 2: Complete Setup & Activation Walkthrough](recordings/02-burpsuite-setup.mp4)**  
> *(Replace the link or filename above with your actual uploaded video file / YouTube link)*

```
[▶ Video 2: Loader Execution, License Activation, and Launcher Setup]
```

---

### Step-by-Step Instructions:

#### Step 2.1: Directory & File Structure
Ensure the following files are present in your working directory:
```
BurpSuitePro-v2026/
├── burpsuite_pro_v2026.3.jar
├── loader.jar
├── burpsuite-logo.png
└── install.sh
```

#### Step 2.2: Launch the Key Loader
Start the license key generator loader:
```bash
java -jar loader.jar
```
Keep this window open during the activation process.

#### Step 2.3: Configure the Startup Command & JVM Arguments
Due to modular encapsulation in modern Java (Java 17+), explicit `--add-opens` flags and the `-javaagent` argument are required:

```bash
java \
  --add-opens=java.desktop/javax.swing=ALL-UNNAMED \
  --add-opens=java.base/java.lang=ALL-UNNAMED \
  --add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED \
  --add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED \
  --add-opens=java.base/jdk.internal.org.objectweb.asm.Opcodes=ALL-UNNAMED \
  -javaagent:$(pwd)/loader.jar \
  -noverify \
  -jar $(pwd)/burpsuite_pro_v2026.3.jar &
```

#### Step 2.4: Activation Flow
1. Copy the license key from **loader.jar** into the Burp Suite license prompt.
2. Select **Manual Activation**.
3. Copy the **Activation Request** from Burp Suite and paste it into the loader's request box.
4. Copy the resulting **Activation Response** from the loader back into Burp Suite.
5. Click **Next** / **Finish** to complete activation.

#### Step 2.5: Create a Global System Launcher (Optional)
To run Burp Suite Pro from anywhere via the terminal:
```bash
# Generate launcher script
cat << 'EOF' > /tmp/burpsuitepro
#!/bin/bash
DIR="/opt/BurpSuitePro" # Update this path to where your files are stored
cd "$DIR"
java --add-opens=java.desktop/javax.swing=ALL-UNNAMED \
     --add-opens=java.base/java.lang=ALL-UNNAMED \
     --add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED \
     --add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED \
     --add-opens=java.base/jdk.internal.org.objectweb.asm.Opcodes=ALL-UNNAMED \
     -javaagent:"$DIR/loader.jar" \
     -noverify \
     -jar "$DIR/burpsuite_pro_v2026.3.jar" "$@" &
EOF

sudo chmod +x /tmp/burpsuitepro
sudo mv /tmp/burpsuitepro /usr/local/bin/burpsuitepro
```

Now you can launch it anytime by running:
```bash
burpsuitepro
```

---

## Troubleshooting

| Issue | Cause | Solution |
| :--- | :--- | :--- |
| `UnsupportedClassVersionError` | Incompatible Java version running the jar | Follow **Heading 1** to switch to OpenJDK 21 via `update-alternatives`. |
| `java.lang.reflect.InaccessibleObjectException` | Missing JVM `--add-opens` flags | Ensure all `--add-opens` parameters listed in Step 2.3 are included. |
| Loader does not attach | Path to `loader.jar` is incorrect | Use absolute path with `-javaagent:/path/to/loader.jar`. |
