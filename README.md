Create an inventory file that matches your vagrant settings

```bash
$ echo "vagrant ansible_ssh_user=vagrant \
 ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key \
 ansible_ssh_host=192.168.33.10 \
 ansible_ssh_port=22" > inventory
```

Provision your machine


```bash
$ ansible-playbook -i inventory devmachine.yml
```