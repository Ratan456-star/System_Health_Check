
# 🩺 Health Check System

A bash-based, menu-driven health check utility for Linux systems that performs real-time checks and sends a detailed system report to **ratan.pandey0309@gmail.com** using Gmail SMTP via Postfix.

---

## 📦 Features

- ✅ Disk usage overview  
- 🧠 Memory & CPU stats  
- ⚙️ Running services list (`systemctl`)  
- 📤 Email reporting to Gmail (via Postfix)  
- 🔐 Secure config using Gmail App Passwords  
- 🧾 Clean, readable terminal output and email format  
- 📜 Simple and interactive bash menu  

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/<your-username>/Menu-Based-Health-Check-System.git
cd Menu-Based-Health-Check-System
```

### 2️⃣ Make Script Executable

```bash
chmod +x menu.sh
```

### 3️⃣ Run It!

```bash
./menu.sh
```

When finished, a system health report will be **automatically emailed to `ratan.pandey0309@gmail.com`**.

---

## 📬 Email Setup (Postfix + Gmail)

> These steps are only required **once**, to configure your machine to send mail via Gmail.

### 🔐 Generate Gmail App Password

1. Go to: [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)  
2. Log into your Gmail.  
3. Create a new app password for:  
   **App** = Mail, **Device** = Other → _Postfix_  
4. Copy the 16-character password.

---

### ⚙️ Install Mail Dependencies

```bash
sudo apt update
sudo apt install mailutils libsasl2-modules
```

---

### 🛠️ Configure Postfix

Edit `/etc/postfix/main.cf` and add:

```ini
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

---

### 🔐 Add Your Gmail Credentials

Create the file `/etc/postfix/sasl_passwd`:

```bash
sudo nano /etc/postfix/sasl_passwd
```

Add this line:

```
[smtp.gmail.com]:587 ratan.pandey0309@gmail.com:<your-app-password>
```

Replace `<your-app-password>` with your actual Gmail app password.

Then secure the file and reload Postfix:

```bash
sudo postmap /etc/postfix/sasl_passwd
sudo chown root:root /etc/postfix/sasl_passwd*
sudo chmod 600 /etc/postfix/sasl_passwd*
sudo systemctl restart postfix
```

---

### ✅ Send Test Mail (Optional)

```bash
echo "Test email from Postfix" | mail -s "Postfix Test" ratan.pandey0309@gmail.com
```

---

## 📂 Sample Output (in Email)

```
=== System Health Report ===
Date: Sun Apr 20 06:24:25 AM UTC 2025

Disk Usage:
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv   22G   11G  9.6G  53% /

Running Services:
- docker.service     active
- nginx.service      active
...

Memory Usage:
Total: 4917MB | Used: 544MB | Free: 3266MB

CPU Usage:
%Cpu(s): 28.6 us, 61.9 sy,  9.5 id
```

---

## 👤 Author

**Ratan Pandey**  
📧 [ratan.pandey0309@gmail.com](mailto:ratan.pandey0309@gmail.com)  
GitHub: [@<your-username>](https://github.com/<your-username>)

---

## 🛡️ License

Licensed under the [MIT License](LICENSE)

---


![Screenshot 2025-04-20 122836](https://github.com/user-attachments/assets/8540cc42-a2e7-49c7-96da-197c5f95cde8)
