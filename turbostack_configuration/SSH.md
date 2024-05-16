---
order: 170
icon: passkey-fill
---
# SSH Key Generation and Access

## Setting Up SSH Client on Windows

### Install OpenSSH Client Feature

Windows 10 and later versions include an optional feature called "OpenSSH Client" that you can install via the Settings app or PowerShell.

1. **Using Settings App:**

   - Open Settings (Press `Windows key + I`), then go to "Apps" > "Optional Features".
   - Click on "Add a feature", search for "OpenSSH Client", and click "Install".
2. **Using PowerShell:**

   - Open PowerShell as an administrator.
   - Run the following command:
     ```powershell
     Add-WindowsCapability -Online -Name OpenSSH.Client
     ```

## Setting Up SSH Client on Linux

### Install the OpenSSH client

Make sure OpenSSH client is installed on your Linux system. If not installed, you can install it using your package manager. For example, on Ubuntu or Debian-based systems, you can use:

```bash
sudo apt-get update
sudo apt-get install openssh-client
```

## Generating a personal key and using SSH

### Step 1: Generate SSH Key Pair

If you haven't already generated an SSH key pair, you can do so using the `ssh-keygen` command in PowerShell. Run the following command:

```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

This command will prompt you to choose a location to save the key pair and optionally set a passphrase.

### Step 2: Copy Public Key to Remote Server

Once the key pair is generated, you need to copy the public key to the remote server. You can use tools like `ssh-copy-id` or manually copy the public key.

This command will prompt you to choose a location to save the key pair and optionally set a passphrase.

Replace `username` and `remote_host` with your username and the hostname or IP address of the remote server:

```bash
ssh-copy-id username@remote_host
```

You'll be prompted to enter your password on the remote server. Once authenticated, your public key will be added to the `~/.ssh/authorized_keys` file on the remote server.

### Step 3: Test SSH Connection

You can now test your SSH connection to the remote server using the following command:

```bash
ssh username@remote_host
```

If everything is set up correctly, you should be logged in to the remote server without being prompted for a password.

### Step 4: Optional Configuration

You can configure your SSH client by editing the `~/.ssh/config` file. This file allows you to set options such as default usernames, identities, and SSH server configurations.

You're all set! You can now securely connect to remote servers using SSH without having to enter a password each time.
