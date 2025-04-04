# GLPI to Outlook Auto Importer

This PowerShell script automates the process of downloading a GLPI service desk `.ics` calendar file and importing its events directly into a named sub-calendar within Microsoft Outlook.

This Script is intended as a workaround. A workaround in two aspects, the first of which is if your organisation is using MS SSO or any other external auth-service that in turn does not allow you to use CalDav or WebCal to synchronize your glpi calendar to your Outlook calendar. The second part of the workaround is simply the fact that the issue can be resolved if you have admin or more specific permission in order to implement a smoother solution; such as creating a user utilizing glpi db username and password that can access your or many other's calendars. The second part would inherently result in Webcal or CalDav synchronization being possible.

---

## 📦 Features

- Downloads an `.ics` file from a configured GLPI URL with token authentication.
- Imports event(s) into a custom calendar in Outlook (e.g., `AutoImport`).
- Automatically creates required folders if they don't exist.
- Logs basic output and validation results to the console.

---

## 📁 Project Structure
``` bash
.
├─── v1-0-x.ps1                               # Working alpha version, current final number varies
├─── data.psd1                                # Configuration values for variables used in script
├───dependencies
|   └─── ics_download.ps1                     # Downloads Ics file from glpi-url specified in data.psd1 
├───InvokeCredentials
|   └─── token_auth_direct_download.ps1       # OLD, no longer in use since authentication does not work due to sso setup
└───tmp                                       # Unused files, that are simple there for test purposes
```

---
## :wrench: Setup

### 1️⃣ Get Your Personal ICS Download Link

1. Visit your GLPI calendar planning page:  
   `glpi-address.xyz/front/planning.php`

2. Locate and copy the download link that's tied to your account.

![image](https://github.com/user-attachments/assets/b5926ea4-262f-44c3-8955-1c4556f4abc5)

---

### 2️⃣ Set Up the `data.psd1` Configuration File

1. Open the configuration file located at /path/to/github-repo/data.psd1:


2. Paste your copied ICS download link into the `IcsUrl` field:

```powershell
@{
    ExpectedCalendarName = 'AutoImport'
    IcsUrl = 'PASTE_YOUR_ICS_URL_HERE'
}
```

---

### 3️⃣ Set the Target Calendar Name

1. In the same configuration file (`data.psd1`), update the `ExpectedCalendarName` value to match the **exact name** of the Outlook calendar you want to import the events into.
- This `ExpectedCalendarName` is going to reference an Outlook calendar, **You can create the calendar later**, just remember the name.

   ```powershell
   @{
       ExpectedCalendarName = 'YourCalendarNameHere'
       IcsUrl = 'https://your-glpi-url...?token=...'
   }
   ```

  2. #### ⚠️ **Important Recommendations**
     Avoid setting this (The calendar name) to your default Outlook calendar (where your personal or work meetings are stored). Instead, create a separate calendar — for example, AutoImport — to isolate imported events. This is recommended because the script deletes all existing events in the calendar before importing the latest ones. This ensures that duplicate entries aren’t created when re-importing to fetch newly added events.

---

### 4️⃣ Create an Outlook Target Calendar for Export

1. Open your Outlook application, preferably the Classic version.  
   Go to the **Calendar** view. In your navigation bar, click **Add Calendar** → **Create New Blank Calendar**:

   ![image](https://github.com/user-attachments/assets/2ec0365c-f709-4713-84cd-033e596124d1)

2. Name the calendar to exactly match the value you set for `ExpectedCalendarName` in your `/path/to/github-repo/data.psd1` file:

   ![image](https://github.com/user-attachments/assets/7796e6d6-cdd1-4bf4-93da-a55db34c9939)


---


## 🚀 Usage

> Before running the script, please ensure the following:

- ✅ **Outlook (preferably Classic) should be open**  
  The script uses the Outlook COM object to create and manage calendar events. If Outlook is closed, the script won't fail to access the required calendar folders, but you will be annoyed 🥲.

- ✅ **The target calendar (e.g. `AutoImport`) must already exist in Outlook**  
  The script does not create new calendars. Be sure to create the calendar manually before running the script.


If the script won't run, use:

```powershell
powershell.exe -ExecutionPolicy Bypass -File .\v1-0-4.ps1
```

---










