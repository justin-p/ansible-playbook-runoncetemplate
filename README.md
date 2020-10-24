# ansible-playbook-runoncetemplate

[![Github Actions](https://img.shields.io/github/workflow/status/justin-p/ansible-playbook-template/CI?label=Github%20Actions&logo=github&style=flat-square)](https://github.com/justin-p/ansible-playbook-runoncetemplate/actions)

A Ansible template for playbooks that you want to run once on a system using a local version of ansible.
Perfect for once off configs such as [this](https://github.com/justin-p/ansible-playbook-terraform-workstation) or [this](https://github.com/justin-p/ansible-playbook-my-linux-workstation).

The included `setup.sh` will do the following:

1. Install Ansible and all the needed dependencies.
2. Clone the repository at `/tmp/$REPO_NAME`
3. Install any required roles for the playbook listed in the `requirements.yml` file.
4. Run the playbook `main.yml` using the `inventory.yml` as its inventory file.

This works great if you curl the script and pipe it to bash (as show in the quick install). 

This template also has Molecule and Github Actions preconfigured to lint, deploy and verify the playbook against Ubuntu 20.04 and 18.04 containers. For local development using Molecule is also recommended. This template uses [geerlingguy's](https://github.com/geerlingguy) [docker images](https://ansible.jeffgeerling.com/) so that it has have full support for creating and restarting services. See [this blogpost](https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule) for more info.

Below is some standard text I include in my playbook repositories:

---

## Installation

### Quick

1. Run `curl https://raw.githubusercontent.com/justin-p/ansible-playbook-runoncetemplate/main/setup.sh | bash`
2. Enter sudo password.
3. Reboot the system.

### Manual

1. `git clone https://github.com/justin-p/ansible-runoncetemplate`
2. `cd ansible-template`
3. `sudo apt-get update -y && sudo apt-get install git curl python3 python3-pip`
4. `pip3 install --user ansible `
5. `export PATH=$PATH:/$HOME/.local/bin`
6. `ansible-galaxy install -r requirements.yml`
7. `ansible-playbook main.yml -i inventory.yml`
8. Reboot the system.

## Local Development

This playbook includes Molecule that will spin up a local docker environment to deploy, configure and test this playbook.

Development requirements:

- Docker
- Molecule
- yamllint
- ansible-lint

or simply use a VM with [this](https://github.com/justin-p/ansible-terraform-workstation) configuration.

### Basic Molecule usage

- Build a container and apply the current playbook: `molecule converge`  
Note, when using converge the container is not destroyed, allowing you to quickly test changes by running this command consecutive times.

- Interact with the containers created by molecule: `molecule login`  
Note, this works great with converge allowing you to peek inside the container and manually check why things didnt work.

- Apply tests defined in the `verify.yml` file: `molecule verify`

- Remove any containers left over from the converge command: `molecule destroy`

- Run the playbook and any included tests against clean containers: `molecule test`

## License

MIT

## Authors

Justin Perdok ([@justin-p](https://github.com/justin-p/))

## Contributing

Feel free to open issues, contribute and submit your Pull Requests. You can also ping me on Twitter ([@JustinPerdok](https://twitter.com/JustinPerdok)).
