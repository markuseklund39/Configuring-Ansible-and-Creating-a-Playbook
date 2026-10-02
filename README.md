# Configuring-Ansible-and-Creating-a-Playbook

Ansible is an example of **Infrastructure as Code (IaC)** and can be used as a configuration management tool. In this setup, Ansible was installed on Arch Linux and a playbook was created to automate the process of creating a new user.

## Installing and Configuring Ansible

Ansible was first installed using the following commands:

```bash
sudo pacman -Syu
sudo pacman -S ansible
```

A project directory, `~/ansible-project/`, was then created along with the configuration file `ansible.cfg` and the inventory file `inventory.ini`. The IP addresses of the client machines were added to the inventory file.

SSH keys were created using:

```bash
ssh-keygen -t ed25519 -C "ansible-key"
```

The keys were then copied to the client machines using `ssh-copy-id`. The path to the SSH keys was specified in the inventory file.

The connection to the clients can be tested with:

```bash
ansible all -m ping --ask-become-pass
```

A successful connection should return a `"ping": "pong"` response from each client.

The uptime of the web servers can also be checked using:

```bash
ansible webservers -a "uptime" --ask-become-pass
```

## Playbook for Creating a New User

A playbook named `create_user.yml` was created to automate the creation of a new user.

A password hash was required and was entered into the playbook. To generate the hash, `mkpasswd` was installed using:

```bash
sudo pacman -S whois
```

The password hash was then generated with:

```bash
mkpasswd --method=sha-512
```

Also, the playbook have to refer to the correct SSH key. The playbook can be executed with:

```bash
ansible-playbook create_user.yml --ask-become-pass
```

After running the playbook, the new user can be verified on the clients using:

```bash
ansible all -a "id username" --ask-become-pass
```
