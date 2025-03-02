### First things to set up servers and environment


create **4 servers** , one the workstations and other 3 servers, then open them in terminal using ssh. 
finally, ssh the other three servers to the worksation using 
```bash
ssh <internal  ip address>
```
use to extract the ip address before ssh 
```bash
ip a
```     
```bash
# On ansible-workstation
ssh-keygen -t rsa -b 4096 -C "ansible-workstation"
# Press Enter to accept default location
# Press Enter twice for empty passphrase (or enter one if you prefer)

# On ansible-workstation
cat ~/.ssh/id_rsa.pub

# On each target server (do this for all 3 servers)
mkdir -p ~/.ssh
echo "paste-workstation-public-key-here" >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

## or copy to the other instances using 
ssh-copy-id -i ~/.ssh/id_rsa.pub user@target_instance_ip

ssh-copy -i ~/.ssh/id_rsa.pub server-1@10.20.0.3

# From workstation, try connecting to each server
ssh username@10.0.0.x  # Replace with actual internal IP

# On each server, ensure correct permissions
ls -la ~/.ssh

# On each server
getenforce
# If it's "Enforcing", you might need to run:
restorecon -R -v ~/.ssh

# On each server
sudo systemctl status sshd

# On each server
sudo cat /etc/ssh/sshd_config | grep PubkeyAuthentication
# Should show: PubkeyAuthentication yes

# On each server
sudo nano /etc/ssh/sshd_config
# Ensure these lines exist and are uncommented:
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

# After changes, restart SSH:
sudo systemctl restart sshd
```
