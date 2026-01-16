To log into your Raspberry Pi (Pi) from a Mac using SSH keys,  
first generate keys on your Mac (if you haven't already),  
then copy your public key to the Pi's `~/.ssh/authorized_keys` file using `ssh-copy-id` or manually,  
and finally, connect via ssh `pi@<your_pi_ip>` from your Mac's terminal to log in password-free. 

## Step 1: Generate SSH Keys on Your Mac (If Needed)

```bash
ssh-keygen -t rsa -b 4096 -C "rpi-patna" -f ~/.ssh/rpi-patna
```
Press Enter to accept default file locations and leave the passphrase empty (or set one for extra security).  
Your public key is usually `~/.ssh/rpi-patna.pub`

## Step 2: Copy Public Key to Raspberry Pi
Using `ssh-copy-id` `(Recommended):

Run   
```bash
ssh-copy-id pi@<your_pi_ip_or_hostname>
```
(e.g., `ssh-copy-id pi@raspberrypi.local` or `ssh-copy-id pi@192.168.1.100`).
Enter the 'pi' user's password when prompted; it will copy the key and set permissions.

## Step 3: Connect from Your Mac

```bash
ssh -i ~/.ssh/rpi-patna ash@192.168.4.183
```
