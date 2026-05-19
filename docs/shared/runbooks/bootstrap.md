# Bootstrap

## Prepare Ansible User

Run this only when a host does not already have the `ansible` user with passwordless sudo.

```sh
sudo -s

id ansible || useradd -s /bin/bash -m ansible
echo 'ansible ALL=(ALL:ALL) NOPASSWD:ALL' | tee /etc/sudoers.d/ansible
chmod 440 /etc/sudoers.d/ansible

mkdir -p /home/ansible/.ssh
cat /path/to/ansible.pub | tee /home/ansible/.ssh/authorized_keys
chmod 600 /home/ansible/.ssh/authorized_keys
chown -R ansible:ansible /home/ansible/.ssh
```

## Run Bootstrap Script

Replace placeholders with the target host and cluster.

```sh
SSH_HOST="<HOST>" \
SSH_PORT="22" \
SSH_PORT_AFTER="55555" \
SSH_USER="root" \
SSH_USER_AFTER="ansible" \
scripts/bootstrap.sh <realm> <platform> <cluster>
```

## Provision

Use project Taskfile groups for standard operations where available. Use `bin/migrate apply` before `bin/run` for a single cluster:

```sh
bin/migrate apply <realm> <platform> <cluster>
bin/run <realm> <platform> <cluster> setup
bin/run <realm> <platform> <cluster> update
```

By convention, dry/check mode is the default. Set `DRY=0` to apply:

```sh
DRY=0 bin/migrate apply <realm> <platform> <cluster>
DRY=0 bin/run <realm> <platform> <cluster> setup
DRY=0 bin/run <realm> <platform> <cluster> update
```
