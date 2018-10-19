# MISP Deployment on RHEL7.X with Ansible
Tested with RHEL 7.5

This Ansible playbook assumes the following:
1. You have set the hostname appropriately on server: `hostnamectl set-hostname {HOSTNAME}`
1. SSH is enabled and accessible on server: `systemctl enable sshd.service && systemctl start sshd.service`
1. You have added an SSH key to `/root/.ssh/authorized_keys` on server (don't forget to remove after!)
1. You have updated all the variables in `hosts` file locally (see below in this file)
1. You have created a GPG key as root (which will be used by MISP) using the command `gpg --gen-key` on server
   * RSA and RSA (default)
   * 4096 bits
   * 0 (key does not expire)
   * Email ***MUST*** match admin_email in `hosts`
   * Set password and remember to configure password in MISP frontend
1. You have registered and attached the system with Red Hat Subscription Manager on server:
   * `subscription-manager register`
   * `subscription-manager attach`

After the above is complete, you're ready to run the playbook: `ansible-playbook -i hosts misp.yml`

Once complete you'll likely want to **remove root's authorized_keys and disable `PermitRootLogin` in `/etc/ssh/sshd_config`**

## `hosts` Variable Definitions
| Variable Name | Description |
| --- | --- |
| ansible_user | User to login to MISP server, must be root |
| ansible_ssh_private_key_file | Full path to SSH private key | 
| mariadb_root_password | Password for root mariadb user |
| mariadb_misp_password | Password for misp mariadb user |
| misp_version_number | MISP latest version number (find on their Github README) |
| timezone | Timezone of MISP server, UTC recommended |
| admin_email | Email for signing GPG key |
| baseurl | https://FQDN_HERE |
| fqdn | IP address OR domain name, ie., misptest.local |
| salt_key | Random string of characters, numbers, and symbols |
| country | Self-signed SSL cert info |
| state | Self-signed SSL cert info |
| locality | Self-signed SSL cert info |
| org | Self-signed SSL cert info |
| org_unit | Self-signed SSL cert info |
| [mispinstance] | Place IP address of your server below this line |
