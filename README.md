# Ansible Role `jm1.known_hosts`

This role helps with managing SSH known hosts files.
If you do not need the functionality to overwrite offending SSH keys (variable `force_overwrite_ssh_known_hosts`),
then you may have a look at OpenSSH's option `StrictHostKeyChecking=accept-new` instead.

*Details*
* SSH host keys will be added to user's SSH known hosts file if is not present in any SSH known hosts file.
* Nothing is done if SSH host keys are present in any SSH known hosts file.
* The role will fail if an offending SSH key has been found in any SSH known hosts file and variable 
  `force_overwrite_ssh_known_hosts` is `no`.
* If an offending SSH key has been found in any SSH known hosts file, but `force_overwrite_ssh_known_hosts` is `yes`,
  then femove old SSH keys will be removed and the offending ones will be added to user's SSH known hosts file.
* The SSH host key is matched against all known hosts files (SSH's options `globalknownhostsfile` and 
  `userknownhostsfile`) which are configured for Ansible's SSH connection.
* SSH host keys will be added to the first known hosts file listed for the user (`userknownhostsfile`).
* If multiple known hosts files have been configured in SSH, then SSH keys will be added to the first user known hosts
  file. This behaviour matches OpenSSH's source file [`sshconnect.c`](
  https://github.com/openssh/openssh-portable/blob/master/sshconnect.c), which says that if host keys get added, then
  they get added to the first file (aka `user_hostfiles[0]`) in the list of hosts files.

**Tested OS images**
- [Cloud images](https://cdimage.debian.org/cdimage/openstack/current/) of `Debian 10 (Buster)` \[`amd64`\]
- [Generic cloud image](https://cloud.centos.org/centos/7/images/) of `CentOS 7 (Core)` \[`amd64`\]
- [Generic cloud image](https://cloud.centos.org/centos/8/x86_64/images/) of `CentOS 8 (Core)` \[`amd64`\]
- [Ubuntu cloud image](https://cloud-images.ubuntu.com/bionic/current/) of `Ubuntu 18.04 LTS (Bionic Beaver)` \[`amd64`\]
- [Ubuntu cloud image](https://cloud-images.ubuntu.com/focal/) of `Ubuntu 20.04 LTS (Focal Fossa)` \[`amd64`\]

Available on Ansible Galaxy: [jm1.known_hosts](https://galaxy.ansible.com/jm1/known_hosts)

## Requirements

None.

## Variables

| Name                              | Default value | Required | Description                                                              |
| --------------------------------- | ------------- | -------- | ------------------------------------------------------------------------ |
| `force_overwrite_ssh_known_hosts` | `no`          | no       | Overwrite SSH known hosts file if Ansible host has an offending SSH key. |

## Dependencies

None.

## Example Playbook

```yml
- hosts: all
  connection: local
  serial: 1 # Prevent concurrent writes to known hosts file
  roles:
    - name: Manage SSH known hosts
      role: jm1.known_hosts
      # Optional: Pass variables to role
      vars:
        force_overwrite_ssh_known_hosts: yes
```

For instructions on how to run Ansible playbooks have look at Ansible's
[Getting Started Guide](https://docs.ansible.com/ansible/latest/network/getting_started/first_playbook.html).

## License

GNU General Public License v3.0 or later

See [LICENSE.md](LICENSE.md) to see the full text.

## Author

Jakob Meng
@jm1 ([github](https://github.com/jm1), [galaxy](https://galaxy.ansible.com/jm1), [web](http://www.jakobmeng.de))
