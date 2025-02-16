# Create a VM in OpenStack with MicroK8S (using Ansible and Terraform)

Creates a number of virtual machines in OpenStack with [MicroK8S](https://microk8s.io/) pre-installed. It uses [MetalLB](https://metallb.universe.tf/) to provide load balancing.

However, as OpenStack only allows [a single floating IP to be assigned to an instance](https://ask.openstack.org/en/question/11901/how-to-configure-multiple-floating-ip-for-one-instance/), this project re-uses the hosts IP (which limits the ports available to Kubernetes).

## Usage

Requires local installation of [Ansible](https://www.ansible.com/) and [Terraform](https://www.terraform.io/)

1. Set required [environment variables](https://www.google.com/search?q=openstack+client+environment+variables) for OpenStack
2. [Override any variable](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#defining-variables-at-runtime) requiring change (see [group_vars/all.yaml](group_vars/all.yaml) for a reference)
3. Run `ansible-playbook deploy.yaml` (and supply your variables; see examples below)
4. Set `KUBECONFIG` to the corresponding generated kubeconfig files and verify that `kubectl get nodes -o wide` works.

## Example

### Pass variables on the command line

```bash
ansible-playbook deploy.yaml -vvv --extra-vars 'instance_count=2 key_name="dev"'
```

### Using a file with variables

Create a file (e.g., `my-vars.yaml`)

```yaml
instance_count: 5
nodes_name: mk8s
key_name: "Dennis 2017"
project_id: 124048b2da0c44e5aee6eca2d180de30
```

Run `ansible-playbook deploy.yaml --extra-vars '@my-vars.yaml'`
