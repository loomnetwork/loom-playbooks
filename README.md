# Loom Playbooks

This repository stores Ansible automation that will help in expediting some of the tasks described in https://loomx.io/developers/

## DappChain

```bash
ansible-playbook -i inventory.yml -vv loom-playbook.yml
```

## Etherboy Automation

### Spinning up instances

1. Set up AWS credentials
2. Run playbook
    ```bash
    ansible-playbook -i etherboy-inventory.yaml etherboy-deploy-instances.yaml --key-file ~/.ssh/etherboy_key.pem -u ubuntu -vv
    ```
3. Collect keypair and save it for example to `~/.ssh/etherboy_key.pem`