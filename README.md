# 🚀 Fully Automated Windows Installation with Pre-Installed Microsoft 365

![Windows Version](https://img.shields.io/badge/Windows-10%2F11-blue.svg)
![Automation](https://img.shields.io/badge/Automation-100%25-brightgreen)
![Pre-Installed Office](https://img.shields.io/badge/Microsoft%20365-Pre--installed-orange)

This project provides an **automated Windows installation** that:
- 🚀 **Installs Windows 10/11 without manual input**
- 🔥 **Removes bloatware & optimizes performance**
- 🖥️ **Pre-installs Microsoft 365 (Office 365 ProPlus Enterprise)**
- 🔒 **Disables telemetry, tracking, and Windows updates**
- ⚡ **Forces local account creation (no Microsoft account required)**

---

## 📖 Table of Contents
### 1. [Features](#features)
### 2. [How It Works](#how-it-works)
### 3. [Setup Instructions](#setup-instructions)
### 4. [Modifications](#modifications)
### 5. [FAQ](#faq)
### 6. [License](#license)

---

## ✨ Features
✅ **Zero manual input** – Fully automated Windows installation  
✅ **Pre-installed Office 365 ProPlus** – Ready to use on first boot  
✅ **Bloat-free setup** – Removes OneDrive, Xbox, Edge, and unnecessary apps  
✅ **Ultra-fast installation** – Uses `autounattend.xml` for speed optimization  
✅ **Privacy-focused** – Disables telemetry, Cortana, and background tasks  
✅ **Optimized for SSDs** – Disables defragmentation & indexing  

---

## ⚙️ How It Works
1. **Windows Installation**  
   - Uses `autounattend.xml` for a fully automated setup.  
   - **No Microsoft account required** (forces local account).  
2. **Optimization & Debloating**  
   - Removes unnecessary apps, disables telemetry, and speeds up Windows.  
3. **Microsoft 365 Installation**  
   - Uses Office Deployment Tool (`setup.exe` + `configuration.xml`) to pre-install Office.  
   - Office is **fully installed before the first login screen**.  

---

## 🔧 Setup Instructions
### **1️⃣ Prepare Your USB Drive**
1. Download the official **Windows 10/11 ISO** from Microsoft.
2. Create a bootable USB using **Rufus** or `Ventoy`.  
   - **For Rufus Users:** Select "Standard Windows Installation."  
   - **For Ventoy Users:** Simply copy the ISO to the USB.  

---

### **2️⃣ Add the `autounattend.xml` File**
1. Copy the `autounattend.xml` file to **`USB:\`**  
   - This ensures Windows Setup will detect it automatically.  
2. Create a folder structure for Office installation:  
- USB:\sources$OEM$$1\Office\
3. Download & extract the **Office Deployment Tool (ODT)** into the `Office` folder.  
- **Download:** [Office Deployment Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49117)  

---

### **3️⃣ Configure Office Installation**
Create a `configuration.xml` inside `USB:\sources\$OEM$\$1\Office\`:
```xml
<Configuration>
 <Add OfficeClientEdition="64" Channel="Current">
     <Product ID="O365ProPlusRetail">
         <Language ID="en-us"/>
         <ExcludeApp ID="OneNote"/>
         <ExcludeApp ID="Teams"/>
     </Product>
 </Add>
 <Display Level="None" AcceptEULA="TRUE"/>
 <Property Name="AUTOACTIVATE" Value="1"/>
 <RemoveMSI/>
</Configuration>
```

✅ This ensures Office is silently installed before the first login.

### **4️⃣ Modify autounattend.xml to Install Office**
Inside autounattend.xml, add this in the oobeSystem phase:
```
<settings pass="oobeSystem">
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
        <FirstLogonCommands>
            <SynchronousCommand wcm:action="add">
                <Order>1</Order>
                <Description>Install Microsoft 365 ProPlus</Description>
                <CommandLine>cmd.exe /c "C:\Office\setup.exe /configure C:\Office\configuration.xml"</CommandLine>
            </SynchronousCommand>
        </FirstLogonCommands>
    </component>
</settings>
```
✅ Office 365 will be fully installed before reaching the desktop.

### **🛠 Modifications**
Want to customize Windows further? Modify autounattend.xml:
- Enable Classic Right-Click Menu (Windows 11)
```
<RunSynchronousCommand wcm:action="add">
    <Order>35</Order>
    <Description>Enable Classic Right-Click Menu</Description>
    <Path>reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f</Path>
</RunSynchronousCommand>
```
- Fully Remove Microsoft Store
```
<RunSynchronousCommand wcm:action="add">
    <Order>36</Order>
    <Description>Remove Windows Store</Description>
    <Path>powershell.exe -ExecutionPolicy Bypass -Command "Get-AppxPackage *WindowsStore* | Remove-AppxPackage"</Path>
</RunSynchronousCommand>
```

## **📜 License**
### MIT License – Free to use, modify, and distribute.
🚀 Credits: Chris Titus Tech (WinUtil Tweaks), Microsoft Office Deployment Tool.

## **⭐ Enjoy a Fully Automated, Pre-Optimized Windows Install!**
🚀 100% Unattended Windows Setup
🔥 Pre-Installed Office 365
💨 Ultra-Fast & Bloat-Free
🖥️ Ready to Use on First Boot!