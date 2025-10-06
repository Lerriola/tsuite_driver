# Running the TSuite Driver with Ansible

## 1. Install Ansible

The TSuite driver requires Ansible. The **preferred and most portable way** is to install it as a Python package via [PyPI](https://pypi.org/project/ansible/).  

If you are using **conda** or a similar environment manager, create your environment first:

```bash
conda create -n tsuite-env python=3.11 -y
conda activate tsuite-env
pip install ansible
```

> ⚠️ Using conda ensures that dependencies are isolated and
avoids conflicts with system packages.

If you prefer to use Ubuntu packages, Ansible can also be installed via apt:

```bash
sudo apt update
sudo apt install -y ansible
```

> Note: pip installation (inside conda) is prioritized because it allows for more recent versions and better environment isolation.

## 2. Configure Your Inventory file

To run ansible-playbook on the ClimateDT VM, ensure that you have a passwordless SSH connection set up. Then, edit your inventory.ini file with the host information and user:

[testing_vm]
<VM_SSH_HOST> ansible_user=<VM_USERNAME> ansible_shell_executable=/bin/bash

Replace <VM_SSH_HOST> and <VM_USERNAME> with your VM’s SSH host and username.

3. Main Configuration Variables

Instead of editing the playbooks directly, modify the main yaml file. This is the only entry point for manual configuration:

main.yml:

# Centralized variables for testing suite playbooks

```yaml
release_version: "5.5.0"
base_dir: "/home/tsuite1/testing_suite_runs"
config_file: "prod_config.yml"  # or main_config.yml
ansible_shell_executable: /bin/bash
```

> This ensures that all playbooks always share the same configuration.

4. Running the Playbooks

Run the testing suite:

```bash
ansible-playbook -i inventory.ini tsuite_run.yml
```

Generate the run error reports:

```bash
ansible-playbook -i inventory.ini tsuite_report.yml
```
