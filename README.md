# Linux Fundamentals & ROS2 Environment Setup

> **Task:** Practicing core Linux/Shell commands + setting up a WSL2 + Ubuntu 22.04 + ROS2 Humble environment

> **Status:** Completed successfully ✅ 

---

## Introduction

This report documents the task assigned in **Basics Linux for Robotics**: getting familiar with Linux and the Shell, then setting up a full **ROS2 Humble** environment on Ubuntu using **WSL2** on Windows.

The report has two parts: a short theory recap of what was covered in the session (Linux basics and shell commands), followed by the practical setup steps with screenshots.

---

## Part 1: Theoretical Overview

### Linux Overview

Linux is a **free, open-source operating system (GNU/Linux)**, built on four layers: **Hardware → Kernel → Shell → Applications**. The Kernel talks directly to the hardware, and the Shell is the interface used to send it commands.

Popular distributions include `Fedora`, `Debian`, `Kali`, `Red Hat`, and `Ubuntu`. **Ubuntu** was chosen for this course because of its strong support within the ROS community and its stability.

### Ubuntu Filesystem — Key Directories

| Directory | Description |
|---|---|
| `/home/username` | User's home directory (equivalent to `C:\Users`) |
| `/usr` | Most installed program files (equivalent to `C:\Program Files`) |
| `/opt` | Optional software — **ROS is installed here by default** |
| `/etc` | System configuration files |
| `/var/log` | Log files |
| `/mnt` | Mount point for drives (used to access Windows files from WSL) |

### Core Shell Commands Practiced

| Category | Commands |
|---|---|
| **Help** | `man <command>` — shows the manual page for a command |
| **Navigation** | `ls`, `ls -la`, `cd <path>`, `cd ..`, `cd ~`, `pwd` |
| **Files & Folders** | `mkdir`, `touch`, `rm`, `rm -r`, `rmdir`, `mv`, `cp`, `nano` |
| **System / Debugging** | `sudo`, `ps` / `ps -A`, `kill <PID>`, `htop`, `dmesg`, `lspci`, `lsusb` |
| **Package Management** | `apt-get update`, `apt-get install/remove <package>` |
| **Power** | `reboot`, `poweroff` |

These commands were practiced hands-on in the terminal and formed the basis for the package-management steps used later in Part 2.

---

## Part 2: Practical Implementation

The setup was done in four stages, using **Windows PowerShell** (as Administrator) for the WSL/Ubuntu install, then the **Ubuntu terminal** for ROS2:

**Stage 1** → Enable WSL2 **Stage 2** → Install Ubuntu 22.04 **Stage 3** → Install ROS2 Humble **Stage 4** → Verify installation

---

### Stage 1: Enabling WSL2

Ran the following command in PowerShell (Administrator):

```powershell
wsl --install
```

The command downloaded and installed the WSL component (v2.7.11) and enabled the `VirtualMachinePlatform` feature. It completed successfully and required a **system reboot** to finish activation, which was then performed.

<p align="center">
  <img src="images/01-wsl-install.png" width="620" alt="Running wsl --install">
</p>
<p align="center"><em>Figure 1: <code>wsl --install</code> completing successfully</em></p>

---

### Stage 2: Installing Ubuntu 22.04

After rebooting, Ubuntu was installed by name (instead of the default distro):

```powershell
wsl --install -d Ubuntu-22.04
```

Ubuntu 22.04 LTS was downloaded and launched automatically, prompting for a default Unix user — created as `rafee` with a password. 

<p align="center">
  <img src="images/02-ubuntu-install.png" width="620" alt="Ubuntu 22.04 installation complete">
</p>
<p align="center"><em>Figure 2: Ubuntu 22.04 installed, user <code>rafee</code> created, Ubuntu prompt active</em></p>

---

### Stage 3: Installing ROS2 Humble

Inside the Ubuntu terminal, the following commands were run in sequence to add the official ROS2 repository and install it:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install software-properties-common curl -y

sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
http://packages.ros.org/ros2/ubuntu jammy main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update
sudo apt install ros-humble-desktop -y
```

Then the ROS setup script was sourced automatically on every new terminal:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

✅ All commands ran successfully with no errors.

---

### Stage 4: Verifying the Installation

The expected verification command didn't work as intended:

```bash
ros2 --version   # ⚠️ did not return a clear result
```

**Workaround:** checked the ROS environment variable instead:

```bash
echo $ROS_DISTRO
```

This returned `humble`, confirming the installation was successful.

<p align="center">
  <img src="images/04-verify-ros-distro.png" width="400" alt="Verifying ROS_DISTRO">
</p>
<p align="center"><em>Figure 4: <code>echo $ROS_DISTRO</code> → <code>humble</code></em></p>

This was the only issue encountered — every other step went smoothly on the first attempt.

---

## Conclusion

A complete ROS2 development environment (**WSL2 + Ubuntu 22.04 + ROS2 Humble**) was successfully set up on Windows. The shell commands practiced in Part 1 made it much easier to follow and troubleshoot the package-management steps in Part 2. The environment is now ready for the next stage of the training.

---

<p align="center"><sub>Report | Rafeef Alotaibi</sub></p>
