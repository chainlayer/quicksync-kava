# quicksync-playbooks
Playbooks to install a Kava node with Chainlayer Quicksync

## Prerequisites
- on your local machine install ansible
- make sure you have ssh-key access to the target machine with either root or a user with sudo rights

## Usage
```
git clone https://github.com/chainlayer/quicksync-kava.git
# edit group_vars/kava.yml file
vim group_vars/kava.yml
# edit ansible-hosts.yml file
vim ansible-hosts.yml
cd quicksync-playbooks
./quicksync
```

## Troubleshooting

#### Cancelled downloads
If for whatever reason the download is cancelled, you can resume it manually (assuming default variables):
```
sudo su - kavauser
cd /home/kavauser
aria2c --continue -x 5 https://get.quicksync.io/kava-3-pruned.20200730.0505.tar.lz4
cd .kvd
tar -I lz4 -xf /home/kavauser/kava-3-pruned.20200730.0505.tar.lz4
exit
# enable and start kava with sudo
sudo systemctl enable kvd
sudo systemctl start kvd
```

### Feedback during downloads
Downloading the blockchain using the quicksync option can take a long time! Ansible will not give you any feedback unfortunately. You can check if aria2c is still working by checking `top` on the host or checking the `aria.log` file in the download dir for progress

### Disk space needed
You need enough harddisk space to store the download as well as the unpacked chain, lz4's compression rate is roughly 30% so you would need at least 2,4 time the download size in disk space. A variable called cl_download_dir can be used to specify a different download directory, that way you could add a temporary second disk for the download and remove it afterwards.

### A word on idempotency
Normally Ansible playbooks are idempotent meaning you can run them multiple times with the same result. This playbook should be idempotent as well however running it a second time will download the whole quicksync another time and is therefore not recommended. 
