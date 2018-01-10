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

See `sample-1-minishift.yml` to install an Openshift in a VM.

## Using the Ansible Role

Install the role:
```
$ sudo ansible-galaxy install chilcano.minishift
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
