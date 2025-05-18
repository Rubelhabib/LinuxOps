# 🔐 SSH Key-Based Authentication with Custom SSH Host Configuration

This document provides a step-by-step guide on how to securely log in using SSH Public Key Authentication and how to configure the `.ssh/config` file for simplified and efficient remote server connections.

---

## 📚 Table of Contents

- [Authentication Methods](#authentication-methods)
- [Advantages of Public Key Authentication](#advantages-of-public-key-authentication)
- [Best Practices](#best-practices-for-public-key-authentication)
- [Step-by-Step Setup](#step-by-step-setup)
  - [Step 1: Generate SSH Key Pair](#step-1-generate-ssh-key-pair)
  - [Step 2: Copy Public Key to Remote Host](#step-2-copy-public-key-to-remote-host)
  - [Step 3: Test SSH Connection](#step-3-test-ssh-connection)
  - [Step 4: Create SSH Config File](#step-4-create-ssh-config-file)
  - [Step 5: Connect Using Host Alias](#step-5-connect-using-host-alias)
- [Architecture Diagram](#architecture-diagram)
- [Conclusion](#conclusion)

---

## 🛡️ Authentication Methods

SSH supports two types of authentication:

1. **Password-based authentication**
2. **Public key-based authentication** ✅ (Recommended)

Public key authentication is more secure than passwords and is also ideal for automation and scripting.

---

## ✅ Advantages of Public Key Authentication

- **Security:** No password transmission, making it more secure.
- **Convenience:** No need to type your password every time.
- **Automation:** Ideal for use in scripts and DevOps workflows.
- **Hard to Crack:** Much less vulnerable to brute-force attacks.

---

## ⚠️ Best Practices for Public Key Authentication

- Never share your **private key** with anyone.
- Use a **passphrase** on your key for added protection.
- Only add **specific public keys** to the `authorized_keys` file.
- **Rotate** your keys regularly.

---

## 🚀 Step-by-Step Setup

### 🧩 Step 1: Generate SSH Key Pair

```bash
ssh-keygen -t rsa -b 2048
```

- Public Key: `~/.ssh/id_rsa.pub`
- Private Key: `~/.ssh/id_rsa`

---

### 📤 Step 2: Copy Public Key to Remote Host

```bash
ssh-copy-id username@remote_ip
```

This command appends your public key to the `~/.ssh/authorized_keys` file on the remote server.

---

### 🔍 Step 3: Test SSH Connection

```bash
ssh username@remote_ip
```

If you log in without being prompted for a password, key-based authentication is successful.

---

### ⚙️ Step 4: Create SSH Config File

```bash
vim ~/.ssh/config
```

Add the following configuration:

```ini
Host web-server
  HostName 192.168.10.93
  User rubel
  Port 8080
  IdentityFile ~/.ssh/id_rsa
```

Here, `web-server` is an alias, allowing you to use `ssh web-server` instead of a full command.

---

### 💻 Step 5: Connect Using Host Alias

```bash
ssh web-server
```

Now you no longer need to type: `ssh -p 8080 rubel@192.168.10.93`.

---

## 🧠 Architecture Diagram

```
+--------------------------+                       +----------------------------+
|      Local Machine       |  SSH Public Key Auth  |        Remote Server       |
|--------------------------|---------------------->|----------------------------|
| ~/.ssh/id_rsa (private)  |  ⬅️ Used to decrypt   | ~/.ssh/authorized_keys     |
| ~/.ssh/config            |     the challenge     | contains the public key    |
+--------------------------+                       +----------------------------+
```

---

## 🏁 Conclusion

By following this guide, you will be able to:

- Generate an SSH Key Pair
- Configure the Public Key on the remote server
- Use the `.ssh/config` file for simplified connections

This will make your SSH connections more **secure, fast, and convenient**.

---

**✨ Happy SSH-ing!**
