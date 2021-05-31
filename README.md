# ansible-print-server
Ansible playbook to setup and manage a Ubuntu print server

## Setup Ansible User

Log into the new install and create the ansible user

```bash
ssh pi@raspberry
sudo adduser --quiet --shell /bin/bash -ingroup sudo ansible
exit
```

From your Ansible host computer copy across your ansible SSH public key

```bash
ssh-keygen -f ~/.ssh/ansible  
ssh-copy-id -f -i ~/.ssh/ansible.pub ansible@<ip-address>
```

Next we need to add the print server to the host.ini file

./inventory/host.ini

```ini
print-server-1 ansible_host=192.168.3.6 ansible_ssh_private_key_file=~/.ssh/ansible ansible_user=ansible hostname=print-server-1
```

## Run Print Server Playbook

Now its time to run the playbook to set up the print server.

```bash
ansible-playbook 01-print-server.yml -i inventory/hosts.ini
```

The playbook will run through the following steps

1. Suspend default pi account
2. Set hostname to the one in the host.ini file
3. Set to the timezone
4. Setup and secure SSH login
5. Update Raspberry pi packages and dist
6. Setup up firewall rules
7. Install required packages
7. Copy across CUPS config and restart service
8. Copy across Samba config and restart service

## Setup a Printer

In my instance I have a Brother HL-2140. You will need to find your equivalent linux AMD architecture CUPS printer driver

Open a browser and navigate to `http:\\<print-server-hostname or IP>:631`

Click on 'Administration' and 'Add Printer'

Use the 'Brother-HL-2140-hpijs-pcl5e.ppd' in the `./roles/print-server/files/` directory when asked for a driver.

**Done!**





