Yes! You can use **Action1's remote scripting** feature to check the **Windows installation date** across all your managed devices. Here’s how:

---

### **Step 1: Create a PowerShell Script**
Since Action1 supports PowerShell, use this script to fetch the original Windows installation date:

```powershell
# Get the original Windows installation date
$os = Get-WmiObject -Class Win32_OperatingSystem
$installDate = $os.ConvertToDateTime($os.InstallDate)

# Output the result (Action1 will capture this)
Write-Output "Windows Installation Date: $installDate"
```

*(This script uses `Get-WmiObject` for compatibility with older Windows versions. For newer systems, you could also use `Get-CimInstance`.)*

---

### **Step 2: Upload the Script to Action1**
1. Log in to your **Action1 dashboard**.
2. Go to **Scripts** → **Add Script**.
3. Paste the PowerShell script above.
4. Set:
   - **Name**: *"Get Windows Installation Date"*
   - **Type**: *"PowerShell"*
   - **Run as**: *"Local System"* (or a privileged account if needed)
   - **Timeout**: *Default (or adjust if needed)*

---

### **Step 3: Deploy the Script to Devices**
1. **Option A (Run on all devices):**
   - Go to **Scripts** → Select your script → **Run on All Devices**.
2. **Option B (Run on specific endpoints):**
   - Go to **Endpoints**, select devices, then **Run Script** and choose your script.

---

### **Step 4: Check Results**
- After execution, go to **Reports** → **Script Execution Logs**.
- Filter by your script name to see the results for each device.
- You can **export the data to CSV** for further analysis.

---

### **Alternative (For Multiple Details)**
If you want **more system info** (like BIOS date, last reboot, etc.), modify the script:

```powershell
$os = Get-WmiObject -Class Win32_OperatingSystem
$installDate = $os.ConvertToDateTime($os.InstallDate)
$lastBoot = $os.ConvertToDateTime($os.LastBootUpTime)

Write-Output "Windows Installation Date: $installDate"
Write-Output "Last Boot Time: $lastBoot"
Write-Output "BIOS Version: $(Get-WmiObject -Class Win32_BIOS).Version"
```

---

### **Notes:**
- Action1 will **only return results for online devices** at the time of execution.
- If a device has been **reimaged**, this date reflects the latest OS install.
- For **manufacturing date**, check BIOS (`Get-WmiObject -Class Win32_BIOS`).

Would you like help setting up a recurring script to track this automatically?
