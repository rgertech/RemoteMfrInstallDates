# RemoteMfrInstallDates
powershell script to check laptop 
Yes! You can use a **PowerShell script** (like the one for Action1) to check the **laptop's BIOS date**, which often closely matches its **manufacture date**. Here’s how to get both the **Windows installation date** and **BIOS (manufacturing) date** in a single script:

---

### **PowerShell Script to Check BIOS Date & Windows Install Date**
```powershell
# Get Windows Installation Date
$os = Get-WmiObject -Class Win32_OperatingSystem
$installDate = $os.ConvertToDateTime($os.InstallDate)

# Get BIOS Release/Manufacture Date
$bios = Get-WmiObject -Class Win32_BIOS
$biosDate = $bios.ConvertToDateTime($bios.ReleaseDate)

# Output results (Action1 will capture this)
Write-Output "Windows Installation Date: $installDate"
Write-Output "BIOS Release Date (Manufacture Approx.): $biosDate"
Write-Output "BIOS Version: $($bios.SMBIOSBIOSVersion)"
Write-Output "Manufacturer: $($bios.Manufacturer)"
```

---

### **Key Notes:**
1. **BIOS Date ≈ Manufacturing Date**  
   - The BIOS date is typically set when the motherboard is flashed, which happens **close to the laptop's production date**.  
   - This is more reliable than Windows dates (which reset on reinstall).

2. **SMBIOSBIOSVersion**  
   - Some vendors (e.g., Dell, HP) encode the manufacturing week/year in the BIOS version (e.g., `A12 2023-05-01`).

3. **Limitations**  
   - If the BIOS has been updated, the date will reflect the **latest update** (not original manufacturing).  
   - Some OEMs (like Lenovo) store the actual manufacturing date in **SMBIOS** (requires deeper queries).

---

### **How to Run This in Action1**
1. **Add the script** to Action1 (as shown in the previous guide).  
2. **Deploy to target devices**.  
3. **Check logs** for the BIOS date alongside Windows install dates.

---

### **Alternative: Check Serial Number for Exact Manufacture Date**
If you need **precise manufacturing info**, use the laptop’s **serial number** (from `Get-WmiObject -Class Win32_BIOS | Select-Object SerialNumber`) and check:
- **Dell/HP/Lenovo’s warranty lookup tool** (e.g., [Dell Support](https://www.dell.com/support/), [HP Check](https://support.hp.com/)).  
- **Third-party tools** like `wmic bios get serialnumber` + OEM lookup.

---

### **Example Output in Action1**
```
Windows Installation Date: 10/15/2023 2:30:00 PM  
BIOS Release Date (Manufacture Approx.): 08/20/2023 12:00:00 AM  
BIOS Version: A1.00.15  
Manufacturer: Dell Inc.
```

This tells you:
- The laptop was likely made **around August 2023** (BIOS date).  
- Windows was installed later (October 2023).  

Would you like help refining the script for a specific brand (e.g., Lenovo/Dell/HP)?
