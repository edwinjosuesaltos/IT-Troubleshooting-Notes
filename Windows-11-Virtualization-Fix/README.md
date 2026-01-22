# 🔧 Fix: Windows 11 Virtualization (VMware/VirtualBox)

Troubleshooting guide to force disable VBS/HVCI and enable native virtualization performance on Windows 11 (24H2).
🛠️ Guide: Preparing Windows 11 (25H2) for Native Virtualization (Disable Hyper-V/VBS/HVCI)

Many IT professionals and students face severe performance issues (lag, audio distortion, "VTx not available" errors) when running virtualization software like VMware, VirtualBox, or EVE-NG on modern Windows versions (especially Windows 11 24H2-25H2).

This happens because Hyper-V, Virtualization-Based Security (VBS), and Hypervisor-Enforced Code Integrity (HVCI) "hijack" the hardware virtualization extensions (VT-x/AMD-V), preventing other hypervisors from accessing the hardware directly.
## Previous failed method

**Reference:**

For a full discussion of traditional Hyper-V disabling methods, see this article:

[NAKIVO Blog – Uninstalling or Disabling Hyper-V](https://www.nakivo.com/blog/uninstalling-or-disabling-hyper-v-in-windows-10/?utm_source=chatgpt.com)

## 🚀 The Solution: Step-by-Step Guide

*Follow these steps exactly to completely purge the virtualization locks.*

### Step 1: Download the Script

We need the official Microsoft hardware readiness tool to strip the UEFI variables.

1. Download **`DG_Readiness_Tool_v3.6.ps1`**.
    - *(Note: Search* **Device Guard and Credential Guard hardware readiness tool and download***)*.

      <img width="2048" height="1003" alt="image" src="https://github.com/user-attachments/assets/621eadbc-283c-4f02-8201-19a61ee6c6ae" />
      
2. Save it to a known folder (e.g., `C:\Users\YourName\Downloads\dgreadiness`).

**Tip:** Verify the file exists by typing `dir`. You should see `DG_Readiness_Tool_v3.6.ps1`.
<img width="1192" height="453" alt="image" src="https://github.com/user-attachments/assets/40e58d8f-913e-4a74-a40b-399283243891" />

### Step 2: Allow Script Execution\

### 1️⃣ Open PowerShell as Administrator

*This is not a command, but a required system action.*

- Press **Windows Key**.
- Type `PowerShell`.
- Right-click **Windows PowerShell** → **Run as Administrator**.

### 2️⃣ Navigate to the Script Folder

Change the directory to where you downloaded the tool.
*(Replace `YourName` with your actual Windows username)*.

PowerShell

`cd "C:\Users\YourName\Downloads\dgreadiness_v3.6\dgreadiness_v3.6"`

> Verify: Check if the script is present in the folder:
> 
> 
> `dir`
> 

### 3️⃣ Allow Script Execution (Session Only)

This allows the script to run without changing your global Windows security policy permanently.

PowerShell

`Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`

- *Press **Y** (Yes) or **A** (Yes to All) if prompted.*

### 4️⃣ Run the DG_Readiness Tool

Execute the script with the necessary flags to disable all VBS features.

PowerShell

`.\DG_Readiness_Tool_v3.6.ps1 -disable -auto`

- `disable`: Disables Hyper-V, VBS, HVCI, and Device Guard.
- `auto`: Automatically restarts the PC after applying changes.
<img width="553" height="292" alt="image" src="https://github.com/user-attachments/assets/f13374ca-9177-4c9f-8aeb-c0f5f5d22a47" />


- `F3`: Disables Hyper-V, VBS, HVCI, and Device Guard.
- `Any key to restart`: System restart and log in.

### Final Visual Verification (System Information)

**Command:**

PowerShell

`msinfo32.exe`

<img width="508" height="292" alt="image" src="https://github.com/user-attachments/assets/c486d7e5-13ad-47fe-b296-1a47203f38df" />
<img width="1691" height="917" alt="image" src="https://github.com/user-attachments/assets/82b2b3ca-283e-410a-bf16-55a0bb526492" />
<img width="883" height="891" alt="image" src="https://github.com/user-attachments/assets/8e9763c4-d733-41d6-97b8-1c54e200936b" />


![image.png](attachment:f96f406c-1e55-4050-8a48-07659a760f6d:image.png)

![image.png](attachment:a0e4e055-7448-4e00-a605-0b9f2824606a:image.png)
