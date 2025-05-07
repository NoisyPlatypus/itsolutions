"Live" Web pages:
	
 	Educational page: https://noisyplatypus.github.io/itsolutions/index.html
	Landing Page: https://noisyplatypus.github.io/itsolutions/landing-page.html
	Training Module: https://noisyplatypus.github.io/itsolutions/training-module.html
 	Training Module: https://noisyplatypus.github.io/itsolutions/email-template.html


# Phishing Campaign Simulation Setup Guide

This guide will walk you through setting up a complete phishing simulation campaign using GoPhish and MailHog on a Kali Linux virtual machine. This is part of our cybersecurity project assignment.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Initial Setup](#initial-setup)
3. [GoPhish Configuration](#gophish-configuration)
   - [Finding Your VM's IP Address](#finding-your-vms-ip-address)
   - [Setting Up a Sending Profile](#setting-up-a-sending-profile)
   - [Creating an Email Template](#creating-an-email-template)
   - [Building a Landing Page](#building-a-landing-page)
   - [Creating a User Group](#creating-a-user-group)
   - [Launching Your Campaign](#launching-your-campaign)
4. [Tracking Results](#tracking-results)
5. [Troubleshooting](#troubleshooting)

## Prerequisites

- Kali Linux VM installed and running
- GoPhish downloaded from: https://github.com/gophish/gophish/releases/v0.12.1
- MailHog downloaded from: https://github.com/mailhog/MailHog/releases/v1.0.0
- Basic understanding of phishing campaigns and web forms

## Initial Setup

### Starting GoPhish and MailHog

1. Open a terminal in your Kali Linux VM
2. Navigate to your GoPhish directory:
   ```bash
   cd /path/to/gophish
   ```
3. Make the GoPhish binary executable if it isn't already:
   ```bash
   chmod +x gophish
   ```
4. Run GoPhish with appropriate permissions:
   ```bash
   sudo ./gophish
   ```
5. In a separate terminal, navigate to your MailHog directory:
   ```bash
   cd /path/to/MailHog
   ```
6. Make the MailHog binary executable:
   ```bash
   chmod +x MailHog
   ```
7. Run MailHog:
   ```bash
   ./MailHog
   ```

### Accessing Admin Interfaces

- **GoPhish Admin Panel**: https://127.0.0.1:3333 (default credentials: admin/gophish)
- **MailHog Web Interface**: http://127.0.0.1:8025

## GoPhish Configuration

### Finding Your VM's IP Address

Before setting up your campaign, you need to determine your Kali VM's IP address:

1. Open a terminal in your Kali VM
2. Run the following command:
   ```bash
   ip addr show
   ```
   or
   ```bash
   ifconfig
   ```
3. Look for the `inet` address on your active network interface (likely eth0 or wlan0)
4. Note this IP address (e.g., 192.168.x.x) - you'll need it for the campaign URL

### Setting Up a Sending Profile

1. In the GoPhish admin panel, navigate to **Sending Profiles**
2. Click **+ New Profile**
3. Configure the profile with these settings:
   - **Name**: IT Solutions Security Alerts
   - **From**: security-alerts@itsolutions.com
   - **Host**: localhost:1025 (MailHog's default SMTP port)
   - **Interface Type**: SMTP
   - **Ignore Certificate Errors**: Checked
   - Leave username/password blank for MailHog testing
4. Click **Send Test Email** to verify the connection works
5. Check the MailHog interface to confirm receipt
6. Save the profile

### Creating an Email Template

1. Go to **Email Templates** in GoPhish
2. Click **+ New Template**
3. Configure the template:
   - **Name**: Security Alert - Account Verification
   - **Subject**: Your Account Requires Immediate Attention
   - **HTML**: Copy and paste the HTML from the email-template.html file in this repository
   - **Attachments**: None needed for this simulation
   - Ensure `{{.URL}}` placeholder exists in your HTML where the phishing link should go
4. Click **Save Template**

### Building a Landing Page

1. Go to **Landing Pages** in GoPhish
2. Click **+ New Page**
3. Configure the page:
   - **Name**: ITSolutions Login Page
   - **HTML**: Copy and paste the HTML from landing-page.html in this repository
   - **Capture Submitted Data**: Checked
   - **Redirect URL**: https://noisyplatypus.github.io/itsolutions/index.html
4. Click **Save Page**

### Creating a User Group

1. Go to **Users & Groups** in GoPhish
2. Click **+ New Group**
3. Name the group (e.g., "Phishing Test")
4. Add test users:
   - You can import from CSV or add manually
   - For testing, add your own email address or create test accounts
5. Click **Save Group**

### Launching Your Campaign

1. Go to **Campaigns** in GoPhish
2. Click **+ New Campaign**
3. Configure the campaign:
   - **Name**: ITSolutions Security Test
   - **Email Template**: Select your "Security Alert - Account Verification" template
   - **Landing Page**: Select your "ITSolutions Login Page"
   - **URL**: Enter `http://YOUR-VM-IP-ADDRESS` (use the IP address you found earlier)
   - **Launch Date**: Schedule or launch immediately
   - **Sending Profile**: Select "IT Solutions Security Alerts"
   - **Groups**: Select your test user group
4. Click **Launch Campaign**

## Tracking Results

After launching your campaign, you can monitor its progress in real-time:

1. Go to the **Campaigns** dashboard in GoPhish
2. Click on your active campaign
3. View statistics on:
   - Emails sent/opened
   - Links clicked
   - Credentials submitted
   - Users who visited the educational page
4. Click on individual results to see detailed information about each target's interaction with the campaign

## Troubleshooting

### Common Issues and Solutions

1. **Cannot access GoPhish admin panel:**
   - Ensure GoPhish is running (check terminal output)
   - Try using http://127.0.0.1:3333 instead of https

2. **Emails not showing in MailHog:**
   - Verify MailHog is running (check terminal output)
   - Confirm the sending profile host is set to localhost:1025

3. **Phishing link not working:**
   - Check that you're using your VM's correct IP address
   - Ensure port 80 is not blocked by a firewall
   - Verify target can access your VM's IP address

4. **Landing page not capturing credentials:**
   - Verify "Capture Submitted Data" is checked in landing page settings
   - Check that form input fields have the correct name attributes (email, password)

5. **Redirect to educational page not working:**
   - Confirm you've set the correct redirect URL in landing page settings
   - Ensure the educational page URL is accessible

### Testing Your Campaign End-to-End

Before launching to your test group, it's recommended to send a test email to yourself:

1. In the campaign screen, use the "Send Test Email" option
2. Check MailHog for the test email
3. Follow the link in the email to ensure the landing page loads
4. Submit test credentials
5. Verify you're redirected to the educational page
6. Check GoPhish dashboard to confirm credential capture

---

## Notes for Assessors

This simulation is designed for educational purposes only, as part of the Holmesglen Institute Cyber Security assessment. The campaign includes:

1. An email template creating a sense of urgency
2. A convincing landing page requesting credentials
3. An educational page explaining the phishing test
4. A follow-up training module to improve security awareness
