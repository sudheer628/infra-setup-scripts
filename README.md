# infra-setup-scripts
this repository has collection of various infra setup related scripts

## setup
clone this repo in $HOME directory, rest works fine. this repo has .ansible/hosts which consider that hosts are configured in $HOME/infra-setup-scripts/.ansible/hosts

Here are the steps to set up passwordless SSH authentication:

### Step 1: Generate an SSH Key Pair on the Ansible Control Node

1. **Open a terminal** on the Ansible control node.

2. **Generate an SSH key pair** (if you don’t already have one) by running:

   ```bash
   ssh-keygen -t rsa -b 2048
   ```
   
   - When prompted to "Enter file in which to save the key," press Enter to use the default file location.
   - When prompted for a passphrase, you can either enter a passphrase for added security or press Enter for no passphrase.

### Step 2: Copy the Public Key to the Remote Machine

There are a couple of ways to copy the public key to the remote machine:

#### Option 1: Using `ssh-copy-id`

If you have `ssh-copy-id` available, use it to copy the public key:

```bash
ssh-copy-id user@remote-machine
```

Replace `user` with the remote username and `remote-machine` with the remote machine's IP address or hostname.

#### Option 2: Manual Copy

If `ssh-copy-id` is not available, you can manually append the public key to the `~/.ssh/authorized_keys` file on the remote machine:

1. **Display your public key** with:

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

2. **Copy the output** of the above command.

3. **Log in** to the remote machine.

4. **Edit the `authorized_keys` file**:

   ```bash
   nano ~/.ssh/authorized_keys
   ```

5. **Paste the public key** into this file and save it.

### Step 3: Test Passwordless Authentication

Test that you can SSH into the remote machine without a password:

```bash
ssh user@remote-machine
```

If set up correctly, you should not be prompted for a password.

### Step 4: Configure Ansible to Use the SSH Key

In your Ansible playbook or in the Ansible configuration file (`ansible.cfg`), ensure that Ansible is configured to use the correct private key. For example, in a playbook, you might have:

```yaml
---
- hosts: all
  remote_user: user
  vars:
    ansible_ssh_private_key_file: /path/to/private/key
  tasks:
    - ...
```

Replace `/path/to/private/key` with the path to your private key on the control node.

### Note:

- The user specified in `ssh-copy-id` or in the Ansible playbook should have the necessary permissions to perform the tasks required by your Ansible playbooks.
- If you're working in a team or in a production environment, it's important to manage these keys securely and follow your organization's security policies.


