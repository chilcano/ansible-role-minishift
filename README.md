# Ansible Role: minishift

An Ansible Role that creates an OpenShift cluster locally using Minishift (https://www.openshift.org/minishift) like Minikube does with Kubernetes.
This Role performs the following tasks:

Performs the following tasks:

- Downloads and installs the specified version or latest of Minishift binary and Docker Machine driver.
- Ables to copy to `$PATH` or uses the latest `oc` binary from `~/.minishift/cache/oc/<VERSION>/<OS>/`.
- Creates a Minishift VM with a OpenShift cluster instance.
- Multiple Minishift VM instances running in the same host (https://github.com/minishift/minishift/issues/1508 and https://github.com/minishift/minishift/issues/1843).

## Prerequisites

- Ansible 2.3+
- Prior to running the role, clear your terminal session of any DOCKER* environment variables.
- `sudo` access in your host is required for installing packages.

## Dependencies

None

### OSX

- [homebrew](https://brew.sh)

#### Mounting /Users to the Minishift VM

When the Minishift VM is started, the `/Users` volume will be mounted to the VM. This is done by setting the environment variable `XHYVE_VIRTIO_9P=true`.

## Linux

- KVM installed and working. The role installs the Docker Machine driver for KVM, but it assumes KVM is already installed, and working.

## Fedora

- Install packages `python2-dnf` and `libselinux-python`.

## Known Issues

Follow the Minishift's issues for further information:
https://github.com/minishift/minishift/issues

## Default Role variables

The default variables are in `defaults/main.yml`.

## Example Playbook

See `sample-1-minishift.yml` file for a example.

```
$ cat sample-1-minishift.yml


---
- name: Install Minishift.
  hosts: Pisc0
  connection: local
  gather_facts: yes
  vars:
    ms_hostname_and_profile: openshift0

  roles:
    - role: ansible-role-minishift
      minishift:
        action_to_trigger: install # [ install | fresh_install | clean ]
        action:
          clean:
            installation: true
            local_repo: false
            local_tmp: false
            dependencies: false
          install: true
          start: true
        post_copy_oc: false
        start_options:
          vm-driver: virtualbox
          iso-url: file:///Users/Chilcano/Downloads/__kube_repo/minishift-b2d-iso/v1.2.0/minishift-b2d.iso
          #iso-url: file:///Users/Chilcano/Downloads/__kube_repo/minishift-centos-iso/v1.2.0/minishift-centos7.iso
          #iso-url: https://github.com/minishift/minishift-b2d-iso/releases/download/v1.2.0/minishift-b2d.iso
          #iso-url: https://github.com/minishift/minishift-centos-iso/releases/download/v1.2.0/minishift-centos7.iso
          profile: "{{ ms_hostname_and_profile }}"
          #openshift-version: ""        # latest (https://hub.docker.com/r/openshift/origin/tags)
          openshift-version: "v3.7.0"
          #openshift-version: "v3.6.0"
          #openshift-version: "v1.5.1"  # v3.5.1
          #openshift-version: "v1.5.0"  # v3.5.0
          show-libmachine-logs: ""
        openshift:
          admin_usr: "system:admin"
          admin_pwd: anypassword
        repo:
          name: minishift/minishift
          release_tag_name: "" # latest
          #release_tag_name: "v1.8.0"
          #release_tag_name: "v1.7.0"
          #release_tag_name: "v1.6.0"
```


## Using the Ansible Role

Install the role:
```
$ ansible-galaxy install chilcano.minishift
```

Copy the playbook from your roles path to the current working directory:
```
$ cp ${ANSIBLE_ROLES_PATH}/chilcano.minishift/sample-1-minishift.yml .
```

Create an `inventory` file
```
$ echo $(hostname) > ./inventory
```

Run the playbook:
```
$ ansible-playbook -i inventory --ask-become-pass sample-1-minishift.yml
```

## Creating and running multiples Minishift instances

Although Minishift is an active project, there is a great work trying to improve several functionalities.
Specifically, run multiple instances locally requires to follow below steps in order get that.

https://github.com/minishift/minishift/issues/1843

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Roger Carhuatocto](https://www.linkedin.com/in/rcarhuatocto), author of [HolisticSecurity.io Blog](https://holisticsecurity.io). It is inspired in the Ansible Role [minishift-up](https://github.com/chouseknecht/minishift-up-role) created by [@chouseknecht](https://github.com/chouseknecht).
