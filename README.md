# WINDOWS SETUP GUIDES
*A collection of setup guides for optimization, privacy enhancement, cloning, and migration of Windows*

The guides:

1. **OPTIMIZE WINDOWS** — Maximize performance and longevity of your battery by removing bloatware, optimizing battery charging, and performing a variety of tweaks to maximize CPU, RAM, network, and drive performance.  Two guides are provided here: one is a list of automated tools, the other a collection of manual steps.

2. **CLONE YOUR WINDOWS DRIVE** — If upgrading your machine's drive, you can clone the existing drive to the new one, then swap out the old drive for the new.

3. **MIGRATE YOUR APPS & ACCOUNT TO A NEW MACHINE** — If you have a new machine and want to migrate your old machine's setup (install apps and user account) to the new one, use this guide.

Consult these guides if setting up a new machine, upgrading to a new OS drive, or if you want to replicate your personal setup on another machine.

## **DISCLAIMER**

Always create a backup or system restore point before you begin.

These guides involve modifying firmware settings, the Windows registry, system services, security features, and drive contents. By their nature, such modifications carry risk: an incorrect step, a hardware difference, or a change in Windows itself can result in data loss, an unbootable system, or reduced security. While these steps have worked reliably on the author's systems, no guide can account for every configuration — proceed only if you are comfortable making these kinds of changes and are prepared to troubleshoot and resolve any issues on your own.

Third-party tools referenced in these guides (WinUtil, Win11Debloat, Sophia Script, GTweak, O&O ShutUp10++, BCUninstaller, DiskGenius, Transwiz, and others) are independent projects, are not affiliated with or endorsed by the author, and are governed by their own licenses and terms. Evaluate them yourself before running them, particularly tools that require elevated privileges or modify security settings.

**These guides are provided "as is," without warranty of any kind, express or implied. Your use of these guides constitutes acceptance of the above and the LICENSE terms, which can be found [here](LICENSE)**.

####

## OPTIMIZE WINDOWS (AUTOMATED METHOD)

If you prefer an automated approach, these are well-regarded tools:

* [**Chris Titus Tech's WinUtil**](https://github.com/ChrisTitusTech/winutil) — The most popular Windows optimization tool on GitHub (58k+ stars as of 2026) and the broadest. It's a GUI launched by one PowerShell command (`irm christitus.com/win | iex`), and it bundles app installation (via WinGet), debloating, tweaks, Windows Update controls, and even a Windows 11 ISO builder. It actually launches O\&O ShutUp10++ from inside itself for privacy tweaks. It's MIT-licensed and creates a restore point automatically before applying changes. There's no per-tweak undo though — you roll back via the restore point. It's the most popular for a reason: it covers \~90% of what most people want in one place.
  → **Use if you just want to clean up a fresh Windows install and don't want to think hard about it.**
* [**Win11Debloat**](https://github.com/raphire/win11debloat)  — Win11Debloat is the focused, simple option. Comparable in Github popularity to WinUtil (53k+ stars as of 2026). It does debloating, telemetry disabling, taskbar/right-click cleanup — and that's it. No app installer, no ISO builder. In 2026 it finally got a GUI (previously PowerShell-only).  The app removal list is configurable via an `Apps.json` file.
  → **Use if WinUtil feels like too much and you only want bloat removed.**
* [**Sophia Script for Windows**](https://github.com/farag2/Sophia-Script-for-Windows) — PowerShell module with 150+ tweaks, also on Github (9k+ stars as of 2026). Uses only officially documented methods. Good for automation and repeatability. It's harder to use (you typically edit a preset file before running) but there's a GUI wrapper. Best fit if you want repeatable, auditable, scripted deployments across multiple machines.
  → **Use if you're deploying across many machines or want auditable, documented-only changes.**
* [**GTweak**](https://github.com/Greedeks/GTweak/releases) **—** A portable GUI utility on Github (1.4k+ stars as of 2026) aimed at creating an "ideal Windows setup" through deep system control. Alongside standard bloatware removal, service management, and telemetry blocking, it includes built-in Windows activation methods (HWID/KMS). It specifically targets core security features, giving options to disable Windows Defender, SmartScreen, UAC, and VBS. Open-source under the BSD-3-Clause license, it operates deeply enough that it typically requires you to disable your antivirus or add exclusions just to run it.
  **→ Use if you are an advanced power user who wants an all-in-one portable tool for aggressive tweaking and OS activation, and you fully understand the risks of disabling built-in Windows security features.**
* [**O\&O ShutUp10++**](https://www.oo-software.com/en/shutup10) — Not Open Source but entirely free tool focused specifically on disabling telemetry and privacy-invasive features. No debloating, no app removal. It's a portable GUI from O\&O Software, an established German company (25+ years). What sets it apart is the color-coded recommendations (green/yellow/red for risk level) and the ability to export/import settings profiles across machines. It's also bundled inside WinUtil, so if you run WinUtil you effectively get this too. → **Use if you only care about telemetry/privacy.**

####

## OPTIMIZE WINDOWS (MANUAL METHOD)

If you prefer a manual approach, here is a collection of performance degraders and steps for fixing them. Optimizations are listed in order from most to least important, though opinions will vary. All optimizations described here are optional.

**Safety first:** Before you start, create a System Restore point (or a full drive image — see CLONE YOUR WINDOWS DRIVE below).

### Bloatware

* **What it is:**  Manufacturers (OEMs) bundle self-branded and partner apps (aka **bloatware**) in the baseline Windows installation that comes with your machine.
* **The Issue:**  While some OEM apps are genuinely useful, many of these apps can be problematic:
  * They’re never used, yet consume storage space.
  * They’re poorly optimized and waste memory and CPU cycles.
  * They’re sending data back to the manufacturer without notifying you.
  * They’re difficult to uninstall due to a Windows feature that allows manufacturers to maintain these apps at the BIOS level (WPBT), such that they reinstall on the next boot even if you uninstalled them.
* **The Fix**: Unless you have a specific need for one of these apps, uninstall them.  You can always re-install them later, from the manufacturer’s website or Windows Store if necessary.  NOTE: **Do consider keeping the battery minding app provided by the manufacturer (see 100% Battery & Battery Lifespan below).**

  * **Disable in BIOS:** Enter BIOS and disable OEM-specific features:
    * **ASUS**
      * BIOS \> Tool \> **ASUS Armoury Crate** \> Disable "Download & Install".
      * BIOS \> Tool \> **MyASUS** \> Disable.
    * **Dell / Alienware**
      * BIOS \> SupportAssist System Resolution \> **BIOSConnect** \> **OFF**.
      * BIOS \> Update, Recovery \> **SupportAssist OS Recovery** \> **OFF**.
      * *Note:* Dell often hides the driver injector under "Windows Capsule Update"—disable that too if you want total manual control.
    * **HP**
      * BIOS \> Configuration \> **HP Application Driver** \> **Disable**.
      * BIOS \> Advanced \> **Install HP PC Hardware Diagnostics** \> **Disable**.
    * **Lenovo**
      * BIOS \> Security \> **Lenovo Service Engine** (older models) \> **Disable**.
      * *Modern ThinkPads:* They usually rely on **System Interface Foundation** driver in Windows rather than BIOS injection, but check **Config \> Network \> UEFI Network Stack** to prevent BIOS-level downloads.
    * **Acer**
      * BIOS \> Main \> **Function Key Behavior** (look for "Acer Care Center" or "User Experience Program"). Acer is trickier; they often bake it into the hidden "Recovery" partition rather than the BIOS itself.

  * **Disable in Registry:** If your BIOS hides these options, you can tell Windows to ignore the WPBT binary injection entirely. *NOTE: This key is not officially documented by Microsoft and its effectiveness varies by Windows build. Treat it as a best-effort mitigation, not a guarantee.*
      1. `regedit.exe` (Administrator)
      2. Navigate to: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager`
      3. Create **DWORD (32-bit)**: `DisableWpbtExecution`
      4. Set Value: `1`

  * **Purge Apps & Services:**  Once the manufacturer’s BIOS control is disabled, uninstall their apps and disable their services.

    * **Uninstall Apps:**
      * Run Windows updates first, until there are no more updates to be had.  This will ensure Windows update won’t re-install an OEM app later.
      * Use [BCUninstaller](https://www.bcuninstaller.com/) to uninstall apps in bulk:
        1. Look for apps with the manufacturer’s name in it, along with any trial versions of apps that you don’t need.
        2. Leave Microsoft apps alone. That’s a separate guide not covered in this document.
        3. Try BCUninstaller’s silent uninstall first, and if that doesn’t work, retry using their regular uninstaller.
        4. Consider uninstalling trial offers such as  **McAfee Antivirus,** as Windows built-in antivirus is good enough for most people.
        5. Reboot machine to clear any pending locked files.
        6. Repeat steps 1 — 5 until clear of unwanted apps.

    * **Disable Services**: Run `services.msc` and look for these common offenders:
      * **Dell:** `Dell SupportAssist`, `Dell Data Vault`, `Dell Client Management Service`.
      * **HP:** `HP Analytics Service`, `HP App Helper`, `HP Touchpoint Analytics` (Major telemetry hog).
      * **Lenovo:** `Lenovo Vantage Service`, `ImControllerService`.
      * **Acer:** `Acer Care Center Agent`, `User Experience Improvement`.
      **General Rules:**
        * For each service found, set **Startup Type** to **Disabled** and then **Stop** them.
        * If it has the manufacturer's name and "Service," "Update," "Support," or "Analytics" in the title - disable it.
        * Watch out for unbranded apps and services, like Acer’s `User Experience Improvement`.  You can use `BCUninstaller` and `services.msc` to find these unbranded apps and services (look for Developer or Manufacturer — they have to identify themselves).

### 100% Battery & Battery Lifespan

* **What it is:**  If a laptop spends most of its time plugged in, the battery will remain charged at 100% most of the time.
* **The Issue:** Maintaining a battery at 100% long-term will shorten the life of the battery. If a laptop is frequently used unplugged, then this may be an acceptable tradeoff of lifespan for portability.  However, if the laptop is usually left plugged in, then lowering the maximum charge level to 60-80% will significantly increase battery longevity.
* **The Fix**: Lower the maximum battery charge level under Windows or the BIOS.

  * **Windows**: Windows 11 has a feature called **Smart Charging**, though not all devices may support it.
    * Limits charge: Stops charging at around 80% to slow battery aging when plugged in for long durations.
    * Automatic activation: Turns on when it detects prolonged plug-in use or high temperatures.
    * Icon indicator: A heart symbol appears on the battery icon in the taskbar when active.
    * Resumes full charge: Automatically allows charging to 100% if the battery drops below 20% or if the system detects you need it for portability

  * **BIOS**
    * **Generic setting**: Try entering the BIOS (usually F2, F10, or Del key at machine power on) and see if a "Battery Limit" option exists there.
    * **Manufacturer-specific settings**

      * **Acer (Battery Charge Limit)**
        * **App:** Open **Acer Care Center or AcerSense**.
        * **Steps:** Click **Checkup** \> **Battery Health**.
        * **Action:** Toggle **Battery Charge Limit** to **On** (locks at 80%).

      * **ASUS (Battery Health Charging)**
        * **App:** Open the **MyASUS** app.
        * **Steps:** Go to **Customization** \> **Power & Performance**.
        * **Action:** Select **Balanced Mode** (limits to 80%) or **Maximum Lifespan Mode** (limits to 60%).

      * **Dell (Custom Charge)**
        * **App:** Open **Dell Power Manager** (or "MyDell").
        * **Steps:** Go to **Settings** \> **Battery Information**.
        * **Action:** Select **Custom**. You can then set "Stop charging at" to **80%**.
        * **Alternative:** Select "Primarily AC Use" which automatically manages the limit.

      * **HP (Battery Care)**
        * **Method A (BIOS):** Most consumer HPs require this.
          * Go to **Configuration** \> **Battery Care Function** (or "Battery Health Manager").
          * Set to **80%** or **Maximize Battery Health**.
        * **Method B (App):** Business models (EliteBook/ZBook) may use the **HP Power Manager** app in Windows.

      * **Lenovo (Conservation Mode)**
        * **App:** Open **Lenovo Vantage** (download from Microsoft Store if missing).
        * **Steps:** Go to **Device** \> **Power**.
        * **Action:** Toggle **Conservation Mode** to **On**.
        * **Result:** Keeps battery at \~60% (best for long-term AC use).

      * **Microsoft Surface (Smart Charging)**
        * **App:** Open the **Surface** app.
        * **Steps:** Go to **Battery & charging** \> **Smart charging**.
        * **Note:** Surface devices usually manage this automatically. You can't always force it unless you enter the UEFI (BIOS) and enable "Kiosk Mode" (Battery Limit), which hard-locks it to 50%.

      * **MSI (Battery Master)**
        * **App:** Open **MSI Center** (or Dragon Center).
        * **Steps:** Go to **Features** \> **System Diagnosis**.
        * **Action:** Select **Balanced** (stops at 80%) or **Best for Battery** (stops at 60%).

### Hibernation

* **What it is:** Hibernation enables fully powering off the system and restoring it to its previous state on power up.  It also enables **Fast Startup**, a related feature that hibernates only the kernel session (not your apps) at shutdown to speed up the next boot.
* **The Issue**: Hibernation permanently allocates disk space equal to 40% of RAM size (the Windows 10/11 default), losing storage space and adding a large write to the drive on every shutdown when Fast Startup is enabled. If you can live without Fast Startup and hibernation, disable them.  Note that putting the machine to sleep, which keeps the RAM powered, is a viable alternative.
* **The Full Fix:** Disable Hibernation and Fast Startup:
  * `cmd.exe` (Administrator): `powercfg /h OFF`
* **The Partial Fix:** If you want to keep Fast Startup but reclaim space, shrink the hibernation file to \~20% of RAM instead:
  * `cmd.exe` (Administrator): `powercfg /h /type reduced`

### Last Accessed Time

* **What it is:** Last accessed time is a file attribute, similar to creation time and last modified time.  It is updated whenever a file is written or read.
* **The Issue**:  Updating this attribute requires writing to the drive, even if just reading a file.   If many files are being searched, every single file in the search causes a write to the drive, slowing down searches.  Most users never utilize last accessed time, or even know about it.
* **The Fix:** Disable "Last Access" Timestamp:
  * `cmd.exe` (Administrator):
    ```fsutil behavior set disablelastaccess 1```
  * **Why disable via User Managed (1) instead of System Managed (3)?** Both values disable last access updates, but System Managed lets Windows override your choice based on system volume size—if your C: drive is ≤128GB, Windows re-enables updates on *all* volumes at boot, including large HDDs. This is particularly problematic when searching file contents on an HDD, where every file touched triggers a write. User Managed (1) keeps the setting disabled regardless of volume size.

### Windows Search Indexing

* **What it is**: A background service that crawls your drive to read file contents so you can search for text inside files.
* **The Issue**: On a machine where a lot of creative work is done (many files, lots of changes), the Indexer must re-scan every changed file, consuming significant CPU and disk I/O.
* **The Fix**: Disable indexing and use a superior search alternative.
  * **Disable Drive Indexing:**
    * Right-click C: Drive \> **Properties**.
    * **Uncheck** "Allow files on this drive to have contents indexed."
    * Select "Apply to Drive, subfolders and files". (Ignore errors for system files).
  * **Exclude Folders:**
    * Settings \> Privacy & security \> Searching Windows \> Set to **"Classic"** or add drive to **Excluded Folders**.
  * **Use these alternatives to Windows Search:**
    * [**WizTree**](https://diskanalyzer.com/) Instant disk space analysis and filename search via MFT
    * [**Everything**](https://www.voidtools.com/support/everything/) Instant filename search via the NTFS MFT; also supports (slower, optional) file content search
    * [**FileLocator Lite or Pro**](https://www.mythicsoft.com/filelocatorpro/download/) Filename and file contents search without indexing
    * [**ripgrep**](https://github.com/BurntSushi/ripgrep) Instant command-line text search
    * [**VS Code**](https://code.visualstudio.com/) File contents search without indexing

### Core Parking

* **What it is:** A Windows feature called **Core Parking** aggressively tries to put CPU cores to "Sleep" (C6 state) when they aren't 100% loaded to save power.
* **The Issue:** When you need that core (e.g., a sudden physics explosion in a game or a compile thread starts), the CPU has to "Wake Up" the core. This takes milliseconds and causes micro-stutters.  Your average FPS might look fine, but your "1% lows" (smoothness) may suffer because the OS is constantly putting cores into a coma and jolting them awake.
* **The Fix:**
  * Set Windows Power Plan to **"High Performance"**.
  * *Note:* Many Windows 11 laptops only expose the "Balanced" plan plus a power-mode slider (Settings \> System \> Power).  On those, set the slider to **Best performance**, or restore the hidden plan with `cmd.exe` (Administrator): `powercfg -duplicatescheme 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c`
  * Optionally, use [**ParkControl**](https://bitsum.com/parkcontrol/) utility to verify "AC Parking" is **Disabled**.

### Web Search & Widgets

* **What it is:** In Windows 11, even if you hide or disable Widgets, Web Search keeps Widget processes suspended in RAM (pre-launch state) to ensure they pop up instantly if you ever change your mind.  These widgets are essentially headless Edge browsers.
* **The Issue:**  `Widgets.exe` and `SearchHost.exe` spawn multiple `msedgewebview2.exe` processes that can consume over 100MB RAM each.  They are literally keeping multiple unseen web browser instances open 24/7 just in case you want to see the weather or search Bing.
* **The Fix:**
  * **Disable Widgets:**
    * `cmd.exe` (Administrator): `winget uninstall "Windows Web Experience Pack"`
    * This fix may be sufficient to get rid of most `msedgewebview2.exe` processes, but try the next two fixes if you want/need to go further.
  * **Disable Web Search:**
    * **Registry:** Create a text file named `DisableWebSearch.reg`, paste this, and run it:
      ```
      Windows Registry Editor Version 5.00
      [HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search]
      "DisableWebSearch"=dword:00000001
      "ConnectedSearchUseWeb"=dword:00000000
      "EnableDynamicContentInWSB"=dword:00000000
      [HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer]
      "DisableSearchBoxSuggestions"=dword:00000001
      ```
  * **Disable Web Search network connection:**
    * Open `powershell` and run:
      ```
      New-NetFirewallRule -DisplayName "Block SearchHost Outbound" -Direction Outbound -Program "%windir%\SystemApps\MicrosoftWindows.Client.CBS_cw5n1h2txyewy\SearchHost.exe" -Action Block
      ```

### Virtualization-Based Security (VBS)

* **What it is:** Virtualization-Based Security (VBS) creates a hidden, isolated virtual machine inside your RAM to host sensitive security kernels. It effectively runs your entire OS on top of a Hypervisor (like running Windows inside a VM).  **Memory Integrity (HVCI)** is the most performance-relevant feature that runs under VBS.
* **The Issue:** This can measurably reduce gaming and compilation performance, though on recent CPUs with hardware support (MBEC — roughly Intel 7th gen / AMD Zen 2 and newer) the overhead of Memory Integrity is small.  The impact is largest on older CPUs.
* **The Fix:** Disable Memory integrity under Core isolation:
  * Go to **Windows Security \> Device Security \> Core isolation**.
  * Turn **Memory integrity** to **OFF**.
  * *Note:* This disables HVCI only, not VBS itself. To check whether VBS is still running, open **msinfo32** and look at **Virtualization-based security** near the bottom of the System Summary. Fully disabling VBS involves additional steps (and may be re-enabled by Windows) - this guide does not cover that.
* **WARNING**: You are trading real security protection (kernel exploit mitigation) for performance.  This tradeoff mainly makes sense on a dedicated gaming machine on older hardware; benchmark before and after to confirm it's worth it on yours.

### Tiny Packets

* **What it is**: Nagle’s Algorithm from 1984 was introduced to prevent networks from clogging up with tiny packets. It forces the OS to wait and "bundle" small data packets into one larger packet before sending.  A related mechanism, **delayed ACK**, can hold acknowledgments for up to 200ms; the two interact badly and cause the lag people notice.
* **The Issue:** These delays can add latency to chatty **TCP** applications like remote desktop and some older online games.  (This does *not* affect UDP traffic at all — and most modern games use UDP, which is why many people see no difference from this tweak.)  Modern applications that care about latency typically already disable Nagle themselves via the per-socket `TCP_NODELAY` option, so the registry tweak below mainly helps older software that doesn’t.
* **The Fix:** Reduce delays in the registry (reboot required):
  1. Open `regedit` to: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`
  2. Click through the folders (GUIDs) until you find the one with your current IP address (look at `DhcpIPAddress`).
  3. Create two **DWORD (32-bit)** values:
     * `TcpAckFrequency = 1` (disables delayed ACK — the value that reliably takes effect)
     * `TCPNoDelay = 1` (historically documented for only some services; many modern Windows applications ignore it)
  4. Expect modest gains at best; this is a low-risk but also low-impact tweak on modern systems.

####

## CLONE YOUR WINDOWS DRIVE

*Clone your main drive to an external one, such that you can swap the clone into your PC (helpful if your main drive dies or you simply want to upgrade capacity or speed), or install the clone in another PC with the same Windows major version (10 or 11\) as the source PC (helpful if you want to move your current Windows setup to a new machine).*

Tools for cloning Windows drives:

**1\. [DiskGenius](https://www.diskgenius.com/)** (Free & Best for Live Cloning) A reliable free choice for "Hot Migration" (cloning while Windows is running).  [Hasleo Backup Suite](https://www.easyuefi.com/backup-software/backup-suite-free.html) is a solid free alternative that also offers System/Disk Clone plus full imaging and backup features.
  * **Why**: Its "System Migration" feature is completely free, supports migrating to smaller or larger drives, and can perform the clone without rebooting the machine.
  * **Cost**: Free.

**2\. [Rescuezilla](https://rescuezilla.com/)** (Open Source / Safest) This is a "Live USB" tool. You put Rescuezilla on a thumb drive, boot from it, and *then* clone your internal drive to your external drive.
  * **Why**: Because it runs outside of Windows, it creates a perfect 1:1 copy without Windows locking files or interfering. It is the user-friendly GUI version of Clonezilla.
  * **Cost**: Free / Open Source.

**3\. Manufacturer Tools (Samsung/WD/Crucial)** If you purchased a branded retail drive, it likely includes a free license for premium cloning software:
  * **Samsung**:        Data Migration Software
  * **WD / SanDisk**:   Acronis True Image WD Edition
  * **Crucial**:        Acronis True Image for Crucial

**4\. General rules:**
  * If your source drive is **BitLocker** encrypted, disable the encryption before cloning or else have your BitLocker Recovery Key handy before getting started.  Note that new Windows 11 machines often ship with **Device Encryption enabled automatically** — check the destination machine too, and back up its recovery key (Microsoft account \> Devices \> BitLocker keys) before swapping drives.
  * **Windows activation:** The Windows digital license is tied to the motherboard, not the drive.  A clone swapped into the *same* machine stays activated, but a clone moved to a *different* machine may need reactivation (OEM vs Retail license).  Before the move, link your license to your Microsoft account (Settings \> System \> Activation); after the move, run the Activation troubleshooter and choose "I changed hardware on this device recently."  OEM licenses may not transfer at all.
  * If you are cloning one machine and installing the cloned drive in a different machine you will likely need to update the drivers. Use **Windows Update** and/or [**Snappy Driver Installer Origin (SDIO)**](https://www.glenn.delahoy.com/snappy-driver-installer-origin/) to update the drivers for you.  SDIO is especially helpful for obtaining drivers that Windows Update misses.
  * If your new cloned drive has more capacity than your old drive, DiskGenius usually handles the resize automatically, but you can also use **diskmgmt.msc** to extend the volume manually after the clone.

####

## MIGRATE YOUR APPS & ACCOUNT TO A NEW MACHINE

*Perform a thorough migration from a Windows 10 machine to Windows 11 machine (without paid tools such as [Laplink PCmover Professional](https://go.laplink.com/product/pcmover-professional)), by following these steps to handle migration of apps, users, and default user to a new machine.*

*NOTE: Simply moving your drive from a Windows 10 to a Windows 11 machine may not work, especially if the new machine doesn’t have Windows 10 drivers.  However, if you are migrating between machines with the same Windows version, then consider simply cloning the old machine drive and installing that in the new machine.  See CLONING WINDOWS DRIVE above for steps to do that.*

*NOTE: Windows 10 reached end of support on October 14, 2025\.  Unless enrolled in Extended Security Updates (ESU), a Windows 10 machine no longer receives security patches — another reason to complete a migration to Windows 11, to avoid keeping the old machine online afterward.*

**Phase 1: Export from the Old Windows 10 PC**

This phase captures your current installed software and your personal user profile:

1. **Generate the App Inventory (apps.json):**
   * Open **PowerShell** (Administrator).
   * Run: `winget export -o C:\apps.json --accept-source-agreements`
   * *Note:* This installs the newest versions on the new PC, use `--include-versions` flag if you want the versions from the old PC.
2. **Back up your User Profile:**
   * Download and run [Transwiz](https://www.forensit.com/move-computer.html).
   * Select **"I want to transfer data to another computer"**.
   * Choose the user profile you want to copy — probably your user account.
   * Save the resulting `.zip` transfer file and your `apps.json` to an external USB drive.
3. **OPTIONAL: Export Your Wifi Profiles (or just re-enter Wifi password on new machine)**
   * Open **Command Prompt** (Administrator).
   * Run: `netsh wlan export profile folder=c:\ key=clear`
   * *Result:* This creates an XML file for every Wi-Fi network you've ever connected to, with the password revealed in plain text, saved to your C: drive. Save these to your USB drive.
4. **OPTIONAL: Export your Printers (or just re-add printers on new machine)**
   Windows has a hidden "Printer Migration Wizard" built-in:
   * Press **Win \+ R** and type `PrintBrmUi.exe`.
   * Select **"Export printer queues and printer drivers to a file"**.
   * This saves all your installed printers and their drivers into a single `.printerExport` file.
5. **OPTIONAL: Export Browser Passwords (or just re-enter passwords on new machine)**
   This encryption is tied to your old user account and usually breaks during a copy. You must export them manually, or re-enter them via your password manager.
   * **Chrome/Edge:** Go to Settings \> Passwords \> (Three Dots) \> **Export Passwords**. Save the `.csv` file to your encrypted USB drive.

**Phase 2: Setup the New Windows 11 PC**

This phase prepares the new OS to receive your data.

1. **Mass-Install Applications:**
   * Plug in the USB drive.
   * Open **PowerShell** (Administrator).
   * Run: `winget import -i D:\apps.json --accept-package-agreements --accept-source-agreements`
   * *Note:* Replace `D:` with the drive letter of your USB. This will automatically download and install your apps.
2. **Create your Local Account:**
   * *Note:* Windows 11 initial setup (OOBE) now requires a Microsoft account and internet connection, and Microsoft has been removing the known local-account bypasses.  Complete setup with a Microsoft account (or a throwaway one), then create the local account afterward as follows.
   * Go to **Settings \> Accounts \> Other Users**.
   * Add a new local user, using the same account name as your old machine. **Do not log into this account yet**.

**Phase 3: Import into the New Windows 11 PC**

This phase moves your personal setup onto the machine and optionally sets it as the standard for other users.

1. **Restore your account with Transwiz:**
   * Run Transwiz on the new PC.
   * Select **"I have data I want to transfer to this computer"**.
   * Browse to the `.zip` file on your USB.
   * When prompted, choose **the** user account you just created. Transwiz will now unpack your desktop, documents, and settings into that account.
   * *Result:* Your old machine’s account is now migrated to your new machine.
2. **OPTIONAL: Change the Default User if desired:**
   * Download [DefProf](https://www.forensit.com/downloads.html).
   * Open **Command Prompt** (Administrator).
   * Run: `defprof <name of account you want to act as the template>`.
   * *Result:* This copies the account profile into the hidden `C:\Users\Default` folder. Every new user account created going forward will now automatically use this default desktop layout, app preferences, and settings.

**Tips:**

* **Winget Sync:** If `winget` shows errors for specific apps (like proprietary tools or system apps), it simply means those apps must be manually installed.
* **Transwiz & Windows 11:** While the tool interface mentions Windows 10, version 1.19 is fully compatible with Windows 11\.
* **Store Apps:** If built-in apps (like Calculator) don't work after the transfer, run this command in **PowerShell**, which forces Windows to re-register Universal Windows Platform (UWP) apps (like Calculator, Photos, and the Store) for a specific user:
  `Get-AppxPackage -AllUsers | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}`

**Advantages of doing this manually:**

1. **Repeatability:** You can use these same steps and tools to set up 5 or 50 more computers without paying for a tool.
2. **Stability:** You aren't dragging old Windows 10 drivers or hardware-specific registry "junk" onto your clean Windows 11 installation.
3. **Modernity:** Using `winget` ensures you are getting the latest versions of your apps, rather than moving old, unpatched versions from the previous machine.
