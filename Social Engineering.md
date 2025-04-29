# Social-Engineer Toolkit (SET) Attack Vectors

## 1) Spear-Phishing Attack Vectors
**Purpose**: Send a crafted email with malicious attachments or links.

**Example**:  
Send a Word document with embedded macro that connects back to your Kali machine.

**How**:
- Choose “1” in SET.
- Select a payload (e.g., `meterpreter/reverse_tcp`).
- Choose `FileFormat Attack Vector`.
- Enter your IP (e.g., 192.168.1.10).
- A malicious `.doc` file is created and you are prompted to send it via email.

---

## 2) Website Attack Vectors
**Purpose**: Clone legitimate websites to harvest credentials.

**Example**:  
Clone the Facebook login page and capture usernames/passwords.

**How**:
- Choose “2” in SET.
- Select `Credential Harvester Attack Method`.
- Choose `Site Cloner`.
- Enter your IP (e.g., 192.168.1.10).
- Enter target URL: `https://facebook.com`
- Victim opens your IP in browser → sees fake Facebook → inputs credentials → stored in a file.

---

## 3) Infectious Media Generator
**Purpose**: Create a USB payload to deliver malware.

**Example**:  
Create a `.exe` file to put on a USB drive.

**How**:
- Choose “3” in SET.
- Select payload type (`Windows Meterpreter Reverse_TCP`).
- File gets saved as `program.exe`.
- You place this on a USB → Victim plugs it in → backdoor activates.

---

## 4) Create a Payload and Listener
**Purpose**: Generate a payload file and automatically set up a Metasploit listener.

**Example**:  
Create a reverse shell `.exe` for Windows.

**How**:
- Choose “4” in SET.
- Select OS: Windows
- Choose payload: `meterpreter_reverse_tcp`
- Enter your IP and port.
- A file like `payload.exe` is generated and SET starts a listener waiting for connections.

---

## 5) Mass Mailer Attack
**Purpose**: Send emails in bulk to targets using spoofed addresses.

**Example**:  
Send an email that appears from `admin@paypal.com`.

**How**:
- Choose “5” in SET.
- Choose email method (e.g., Gmail or own SMTP).
- Enter spoofed email (`admin@paypal.com`), subject, and body.
- Enter recipients (single or list).
- Email is sent using your settings.

> **Note**: Gmail blocks this now unless you configure less secure apps or SMTP manually.

---

## 6) Arduino-Based Attack Vector
**Purpose**: Create payloads that mimic keystrokes (HID attacks).

**Example**:  
Use an Arduino or DigiSpark to type a reverse shell payload.

**How**:
- Choose “6” in SET.
- Select OS.
- SET outputs a script that types commands automatically when the device is plugged in.
- Flash the payload onto a device like a USB Rubber Ducky clone.

---

## 7) Wireless Access Point Attack Vector
**Purpose**: Create a rogue Wi-Fi access point to capture data.

**Example**:  
Set up a fake “Free Wi-Fi” access point.

**How**:
- Choose “7” in SET.
- Configure your Wi-Fi adapter in AP mode.
- Set SSID and credentials.
- Victim connects → You can run MITM or credential harvester attacks.

> **Requires** Wi-Fi adapter that supports monitor/AP mode.

---

## 8) QRCode Generator Attack Vector
**Purpose**: Generate malicious QR codes.

**Example**:  
Make a QR code that opens a fake login page when scanned.

**How**:
- Choose “8” in SET.
- Enter the malicious URL (e.g., your fake Facebook clone).
- A QR image is generated.
- Print or share it — victim scans → browser opens malicious site.

---

## 9) PowerShell Attack Vectors
**Purpose**: Run PowerShell payloads that are obfuscated and AV-evading.

**Example**:  
Create an alphanumeric shellcode injector for Windows.

**How**:
- Choose “9” in SET.
- Select `Alphanumeric Shellcode Injector`.
- Choose payload (`reverse_tcp`).
- Enter your IP/port.
- Output: PowerShell script you can embed in docs or drop in memory.

---

## 10) Third Party Modules
**Purpose**: Load custom or external exploit modules.

**Example**:  
Load a custom phishing toolkit or automated phishing frameworks.

**How**:
- Choose “10”.
- Add your module script to SET’s `modules` folder.
- Use it like built-in SET options.


References:
```
https://www.techtarget.com/searchsecurity/tutorial/How-to-use-Social-Engineer-Toolkit

```
