# Build a Kubernetes cluster using k3s via Ansible

Author: <https://github.com/itwars>

## K3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.4.0+
Master and nodes must have passwordless SSH access

## Usage

Please install collection requirements first.

```bash
ansible-galaxy install -r collections/requirements.yml
```

Please list installed all collections with the following command.

```bash
ansible-galaxy collection list
```

And than please install roles requirements with the following command.

```bash
ansible-galaxy install -r roles/requirements.yml
```

Second, create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/my-cluster
```

Third, edit `inventory/my-cluster/hosts.ini` to match the system information gathered above. For example:

```bash
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[k3s_cluster:children]
master
node
```

If needed, you can also edit `inventory/my-cluster/group_vars/all.yml` to match your environment.

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini --ask-become-pass
```

## Reset

```bash
ansible-playbook reset.yml -i inventory/my-cluster/hosts.ini --ask-become-pass
```

## Kubeconfig

To get access to your **Kubernetes** cluster just

```bash
scp ubuntu@192.168.0.200:~/.kube/config ~/.kube/config
```

```bash
ansible all -i inventory/my-cluster/hosts.ini -a "shutdown now" -b --ask-become-pass
```

Because of [ansible config](./ansible.cfg) file setting, we can use the following command and it will act same as above.

```bash
ansible all -a "shutdown now" -b --ask-become-pass
```

## ulimit for the node

```bash
ulimit -a
```

## Checking date of all nodes

```bash
ansible all -a "date"
```

## Checking memory of all nodes

```bash
ansible all -a "free -h"
```

## Gathers facts about remote hosts

```bash
ansible all -m setup
```

## Outputs a list of matching hosts

Does not execute anything else.

```bash
ansible all --list
```

## New Ansible Role

```bash
ansible-galaxy role init [-h] [-s API_SERVER] [--token API_KEY] [-c] [--timeout TIMEOUT] [-v] [-f] [--offline] [--init-path INIT_PATH] [--role-skeleton ROLE_SKELETON] [--type ROLE_TYPE] [-e EXTRA_VARS] role_name
```
