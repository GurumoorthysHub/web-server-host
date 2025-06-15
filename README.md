# Azure VM Apache Web Server Setup with Bash Script

This project demonstrates the process of provisioning an Azure Linux Virtual Machine and configuring it as a web server using Apache2. The setup includes installing Apache, ensuring it starts on boot, and creating a custom welcome page. This project highlights foundational cloud infrastructure and Linux server administration skills.

## Project Overview

The primary goal of this project is to showcase practical experience with:
*   Microsoft Azure Virtual Machine (IaaS) deployment and management.
*   Basic Linux server administration (specifically for Ubuntu or similar Debian-based systems).
*   Automating web server (Apache2) installation and initial configuration using Bash scripting.
*   Network configuration within Azure, including Network Security Groups (NSGs).

## Prerequisites

Before utilizing the concepts or scripts from this project:
1.  An active Microsoft Azure subscription.
2.  An Azure Virtual Machine (Linux, preferably Ubuntu) already provisioned or ready to be provisioned.
3.  SSH access to the Azure VM.
4.  If using the provided script (`apache_setup.sh`), it should be copied to the VM or the repository cloned.

## Files in this Repository (Suggested)

*   `apache_setup.sh`: A Bash script to automate Apache2 installation and setup.
*   `README.md`: This file.

## Setup Steps (Manual & Scripted Approach)

This project can be approached manually via the Azure portal and SSH, or by using a script to automate parts of the configuration on the VM.

**1. Provision an Azure VM:**
    *   Log in to the Azure Portal.
    *   Create a new Linux Virtual Machine (e.g., Ubuntu Server).
    *   During creation or post-creation, configure its Network Security Group (NSG) to allow inbound traffic on:
        *   Port 22 (for SSH)
        *   Port 80 (for HTTP for Apache)

**2. Connect to the VM:**
    *   SSH into your newly created Azure VM:
      ```bash
      ssh your_username@<YOUR_VM_PUBLIC_IP_ADDRESS>
      ```

**3. Install and Configure Apache2 (Manual Commands):**
    *   Update package lists:
      ```bash
      sudo apt update
      ```
    *   Install Apache2:
      ```bash
      sudo apt install apache2 -y
      ```
    *   Ensure Apache2 service is started and enabled to start on boot:
      ```bash
      sudo systemctl start apache2
      sudo systemctl enable apache2
      ```
    *   (Optional) Create a custom welcome page (example):
      ```bash
      echo "<h1>Welcome to My Apache Server on Azure! - Gurumoorthy S</h1>" | sudo tee /var/www/html/index.html
      ```

**4. (Alternative) Using the `apache_setup.sh` Script:**
    *   Copy the `apache_setup.sh` script (see example below) to your VM or clone this repository.
    *   Make the script executable:
      ```bash
      chmod +x apache_setup.sh
      ```
    *   Run the script:
      ```bash
      sudo ./apache_setup.sh
      ```

**5. Verify the Web Server:**
    *   Open a web browser and navigate to `http://<YOUR_VM_PUBLIC_IP_ADDRESS>`.
    *   You should see the default Apache welcome page or your custom `index.html` content.

## Example Bash Script (`apache_setup.sh`)

```bash
#!/bin/bash

# Exit immediately if a command exits with a non-zero status.
set -e

echo "Starting Apache2 setup script..."

# Update package lists
echo "Updating package lists..."
sudo apt update -y

# Install Apache2
echo "Installing Apache2..."
sudo apt install apache2 -y

# Start Apache2 service
echo "Starting Apache2 service..."
sudo systemctl start apache2

# Enable Apache2 to start on boot
echo "Enabling Apache2 to start on boot..."
sudo systemctl enable apache2

# Create a custom index.html file
# This will overwrite the default Apache page if it exists
CUSTOM_HTML_MESSAGE="<h1>Welcome to My Apache Server on Azure!</h1><p>This page was set up by the apache_setup.sh script by Gurumoorthy S.</p>"
echo "Creating custom index.html..."
echo "$CUSTOM_HTML_MESSAGE" | sudo tee /var/www/html/index.html

# Output status
echo "Checking Apache2 status..."
sudo systemctl status apache2 --no-pager # --no-pager prevents interactive mode

echo "---------------------------------------------------------------------"
echo "Apache2 installation and setup complete."
echo "Access your web server at http://<YOUR_VM_PUBLIC_IP_ADDRESS>"
echo "Replace <YOUR_VM_PUBLIC_IP_ADDRESS> with the actual public IP of your VM."
echo "---------------------------------------------------------------------"
