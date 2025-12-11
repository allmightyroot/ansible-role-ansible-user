# Ansible Role: ansible-user

[![CI](https://github.com/allmightyroot/ansible-role-ansible-user/workflows/CI/badge.svg)](https://github.com/allmightyroot/ansible-role-ansible-user/actions)

## Description

Creates and configures an Ansible management user for infrastructure automation. This role sets up a dedicated user account for Ansible-based provisioning and management, with SSH public key authentication and optional passwordless sudo access.

**Key responsibilities:**
- Creates a dedicated ansible management user
- Configures SSH public key authentication
- Sets up sudo access (with or without password)
- Ensures proper permissions and security

## Requirements

- Ansible >= 2.9
- `community.general` collection >= 5.0.0
- **IMPORTANT**: `ANSIBLE_SSH_PUBLIC_KEY` environment variable must be set before running this role

## Role Variables

All variables are prefixed with `ansible_` and can be found in `defaults/main.yml`:

- `ansible_user_name: "ansible"` - Name of the management user to create
- `ansible_ssh_public_key` - **REQUIRED**: SSH public key for the user (must be provided)
- `ansible_user_passwordless_sudo: true` - Grant passwordless sudo to the user

## Required Environment Variables

Set before running the playbook:

```bash
export ANSIBLE_SSH_PUBLIC_KEY="ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI..."
```

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Create ansible management user
  hosts: all
  become: yes

  roles:
    - role: ansible-user
      vars:
        ansible_user_name: ansible
        ansible_ssh_public_key: "{{ lookup('env', 'ANSIBLE_SSH_PUBLIC_KEY') }}"
        ansible_user_passwordless_sudo: true
```

## Usage with Packer

When using with Packer, set the environment variable before running the build:

```bash
export ANSIBLE_SSH_PUBLIC_KEY="$(cat ~/.ssh/id_ed25519.pub)"
packer build .
```

## Testing

To test this role locally:

```bash
export ANSIBLE_SSH_PUBLIC_KEY="ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI..."
ansible-playbook tests/test.yml -i tests/inventory
```

To test with Molecule (if configured):

```bash
export ANSIBLE_SSH_PUBLIC_KEY="ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI..."
molecule test
```

## License

MIT-0

## Author Information

Created by allmightyroot
