# Installing Splunk Enterprise

## Description
<p>This is a detailed step-by-step guide on how to install Splunk on your computer. It will walk you through the entire process, from verifying system requirements and downloading the appropriate installation package for your operating system to completing the setup and accessing the Splunk Web interface. Whether you’re using Windows, macOS, or Linux, this guide ensures a smooth installation experience tailored to your platform.</p>

## System Requirements

1. **Operating System**: Splunk supports various OS platforms:
   - **Windows**: Minimum Windows 10 (64-bit) or Windows Server 2016/2019.
   - **Linux**: Multiple distributions (e.g., CentOS, Ubuntu, Red Hat Enterprise Linux).
   - **macOS**: Supported for limited purposes, such as development or testing.

2. **Hardware Requirements**:
   - **Processor**: 64-bit CPU.
   - **RAM**: Minimum 4 GB (8 GB or more recommended for production).
   - **Disk Space**: At least 20 GB free disk space. Ensure additional storage for logs and data ingestion.

3. **Web Browser** (for the Splunk web interface):
   - Modern browsers like Chrome, Firefox, or Edge are recommended.

## Account Requirements

- **Splunk Account**: Create a free Splunk account on their [official website](https://www.splunk.com/) to access the download.
- **License**:
  - Free Trial: Provides limited features with up to 500 MB/day of data ingestion.
  - Enterprise: Requires a valid license for full-feature access.

## Download Steps

1. Visit the Splunk Download Page.
<p align="center">
<img src="https://i.imgur.com/6tqm9Qj.png" height="80%" width="80%" alt="Splunk"/>
</p>

2. Choose the appropriate Splunk product and version:
   - Splunk Enterprise
   - Splunk Cloud
   - Splunk Universal Forwarder
     
3. Log in or create a Splunk account.
<p align="center">
<img src="https://i.imgur.com/VOwvmSr.png" height="80%" width="80%" alt="Splunk"/>
</p>

4. Select your platform (Windows, Linux, or macOS) and download the installation package.
<p align="center">
<img src="https://i.imgur.com/8ozBV8F.png" height="80%" width="80%" alt="Splunk"/>
</p>

## Windows Installation Instructions

<p>For this tutorial you will install Splunk Enterprise using the default installation settings, which run the software as the Local System user, admin.</p>

1. Navigate to the folder or directory where the installer is located.
<p align="center">
<img src="https://i.imgur.com/QhdoUtm.png" height="80%" width="80%" alt="Splunk"/>
</p>

2. Double-click the ``splunk.msi`` file to start the installer.

3. In the Welcome panel, read the License Agreement and click Check this box to accept the license agreement.
<p align="center">
<img src="https://i.imgur.com/s8kbpo0.png" height="80%" width="80%" alt="Splunk"/>
</p>

4. Click Next.

5. A terminal window appears and you are prompted to specify an administrator userid and password to use with the Splunk Trial.
   > The password must be at least 8 characters in length. The cursor will not advance as you type.
   > Make note of the userid and password. You will use these credentials to login Splunk Enterprise.
<p align="center">
<img src="https://i.imgur.com/07a07zh.png" height="80%" width="80%" alt="Splunk"/>
</p>

6. Click Next.

7. (Optional) You are prompted to create a shortcut on the Start Menu. If you want to do this, click Create Start Menu shortcut.
<p align="center">
<img src="https://i.imgur.com/teCoRI2.png" height="80%" width="80%" alt="Splunk"/>
</p>

8. Click Install.
<p align="center">
<img src="https://i.imgur.com/oduURaL.png" height="80%" width="80%" alt="Splunk"/>
</p>

9. In the Installation Complete panel, confirm that the Launch browser with Splunk check box is selected.
<p align="center">
<img src="https://i.imgur.com/QVrOE9u.png" height="80%" width="80%" alt="Splunk"/>
</p>

10. Click Finish.
    > The installation finishes, Splunk Enterprise starts, and Splunk Web launches in a browser window.
<p align="center">
<img src="https://i.imgur.com/SZeoE88.png" height="80%" width="80%" alt="Splunk"/>
</p>

