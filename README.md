# Automated build of HA k3s Cluster with `kube-vip`

This playbook will build an HA Kubernetes cluster with `k3s` & `kube-vip` via `ansible`.

## 🚀 Getting Started

### 📝 Preparation

1. You will need Ansible installed on your machine.
2. You should have passwordless SSH access to `server` and `agent` nodes
  - otherwise you can supply `--ask-pass --ask-become-pass` arguments to provide credentials
  for each command
3. You will need (ideally) 5 machines in one local network, and each of them
  - should be running one of the following OS
    - Debian
    - Ubuntu
    - CentOS
  - should be on one of the processor architectures
    - x64
    - arm64
    - armhf
4. Copy `inventory/sample` directory
```bash
cp -R inventory/sample inventory/my-cluster
```
5. Edit `inventory/my-cluster/hosts.ini` to match your environment
  - if multiple hosts are in the master group, the playbook will automatically set up
  k3s in [HA mode with etcd](https://rancher.com/docs/k3s/latest/en/installation/ha-embedded/)
6. Edit `inventory/my-cluster/group_vars/all.yml`
  - Especially put your attention to k3s token. If it isn't changed, your internal
  kubernetes network can be considered as compromised (because this secret is publicly stored
  in this repo)

### ☸️ Create Cluster

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini
```

After deployment control plane will be accessible via virtual ip-address
which is defined in inventory/group_vars/all.yml as `apiserver_endpoint`.

## 🔥 Remove k3s cluster

```bash
ansible-playbook reset.yml -i inventory/my-cluster/hosts.ini
```

>You should also reboot these nodes due to the VIP not being destroyed

## 📄 Kube Config

To copy your `kube config` locally so that you can access your **Kubernetes** cluster run:

```bash
scp your-user@ip-of-the-master:~/.kube/config ~/.kube/config
```

## 🔨 Testing your cluster

See the commands [here](https://docs.technotim.live/posts/k3s-etcd-ansible/#testing-your-cluster).

## ⚙️ Troubleshooting

Be sure to see [this post](https://github.com/techno-tim/k3s-ansible/discussions/20) on how to troubleshoot common problems.

## 🙏 Credits

This repo is just a fork.
It wouldn't been possible without these repos and ✨awesome✨ people:

- [techno-tim/k3s-ansible](https://github.com/techno-tim/k3s-ansible)
- [212850a/k3s-ansible](https://github.com/212850a/k3s-ansible)
- [k3s-io/k3s-ansible](https://github.com/k3s-io/k3s-ansible)
- [geerlingguy/turing-pi-cluster](https://github.com/geerlingguy/turing-pi-cluster)
