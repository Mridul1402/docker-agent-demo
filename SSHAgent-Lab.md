## We can use below steps to set up lab machine as slave agent. 
### Step 1: Generate SSH Key Pair for `labuser`

Run the following as the `labuser` user:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
```

* `-t rsa`: Use RSA algorithm
* The full form of RSA is Rivest, Shamir, Adleman. It is named after its inventors, Ron Rivest, Adi Shamir, and Leonard Adleman.
* RSA is a public-key cryptosystem widely used for secure data transmission, particularly over the internet. 
* `-b 4096`: 4096-bit key
* `-f ~/.ssh/id_rsa`: File to save the key
* `-N ""`: No passphrase

This will generate:

* Private key: `/home/labuser/.ssh/id_rsa` **(With Jenkins)**
* Public key: `/home/labuser/.ssh/id_rsa.pub` **(With Slave Machine)**

<img width="730" height="280" alt="image" src="https://github.com/user-attachments/assets/580fc7ce-a5fb-4c70-96cd-7f241bad39f8" />


---

### Step 2: Allow SSH to Itself

Append the public key to `authorized_keys`:

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
```
chmod 600 ~/.ssh/authorized_keys
```
```
chmod 700 ~/.ssh
```

Test the SSH connection using **your public IP**:

```bash
ssh labuser@YOUR_IP_ADDRESS
```

If it logs in **without a password**, you're good to go.

---

### Step 3: Add Private Key in Jenkins

1. Open Jenkins UI
2. Go to: **Manage Jenkins → Credentials → Global → Add Credentials**
3. Select:

   * **Kind**: SSH Username with private key

   * **Username**: `labuser`

   * **Private Key**: "Enter directly"

   * Paste the contents of:

     ```bash
     cat ~/.ssh/id_rsa
     ```

   * Add description: e.g., `labuser-ssh-key`
   * ID: ssh-key

---

### Step 4: Continue Jenkins Agent Setup

Now follow the earlier steps:

* Add a new node, Name: ssh
* Use launch method: **"Launch agents via SSH"**
* Label: ssh
* Host: your public IP
* Remote directory: `/home/labuser/jenkins-agent`
* Select Method Lanuch By SSH
* Select Host as Public IP of your Lab
* Credentials: Select `labuser-ssh-key`
* Select Host Key Verification Strategy as  - No verifying Verification strategy


---

