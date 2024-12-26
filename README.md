# 🔐 Born2beroot

**Born2beroot** is a project focused on system administration and virtualization. The objective is to set up a secure virtual server with strict requirements, demonstrating knowledge of system configuration, security practices, and service management.

---

## 🛠️ Features

### **Mandatory Part**
- **Virtual Machine Setup**:  
  - Create and configure a virtual server using virtualization tools (VirtualBox or UTM).
  - Install a Debian-based or CentOS-based operating system.
- **User Management**:  
  - Implement a robust password policy.
  - Create groups and users with specific permissions.
- **Services**:  
  - Install and configure essential services like SSH.
  - Enable and manage the UFW or FirewallD firewall.
- **System Monitoring**:  
  - Set up and enable the monitoring of system logs and resource usage.
  - Configure a cron job to send system statistics to the root user.

### **Bonus Part**
- **Partition Configuration**:  
  - Set up partitions to achieve a structure similar to the following:  
    - `/`  
    - `/boot`  
    - `/var`  
    - `/home`  
    - `/srv`  
- **WordPress Setup**:  
  - Configure a fully functional WordPress website using the following services:  
    - **lighttpd** as the web server.  
    - **MariaDB** as the database management system.  
    - **PHP** for server-side processing.  
- **Additional Service**:  
  - Set up **LiteSpeed** as an additional service to improve server performance and flexibility. During the defense, you will justify the choice of this service.

---

## 🎯 Objectives

- Learn the essentials of system administration and virtualization.
- Configure a secure and efficient server environment.
- Gain experience in partition management and web server configuration.
- Enhance the server with additional services and justify their use.

---

## 🚀 Usage

### Setting Up the Virtual Machine
1. Install **VirtualBox** or **UTM** on your host system.
2. Download the ISO for your chosen operating system (Debian or Rocky).
3. Follow the setup instructions to create a new virtual machine.

### WordPress and LiteSpeed Configuration
1. Install **lighttpd**, **MariaDB**, and **PHP** to set up the WordPress website.
2. Install **LiteSpeed** as an additional service for enhanced performance.
3. Ensure all services are properly configured and secured.

---

## 📜 License

This project is part of the 42 School curriculum. All rights reserved.
