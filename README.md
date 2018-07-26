# ocp_install_bigip_controller

Installs the bigip controller on OCP.

## Configuration

| Name | Description | Default |
|---|---|---|
| ocp_token | A service a count token with cluster-admin role. | |
| bigip_partition_route_domain | The default Route Domain to assign to the Partition. | 0 |
| bigip_username | The username used to login to Big IP. |
| bigip_password | The password used to login to Big IP. |

### Example of use

Create a file site.yml as follows:

```yaml
- hosts: localhost
  roles:
    - ocp_install_bigip_controller
```
     
Deploy this role in a roles folder under site.yml.

Execute as follows:

```bash
$ ansible-playbook site.yml
```  
 
<a name="ocp-token"></a>
### How to create a token for a service account

```bash
# log in as sys admin
$ oc login -u system:admin

# creates a service account
$ oc create sa automator -n openshift-infra

# adds the account to the admin role
$ oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:openshift-infra:automator

# gets the value of the token using the name
$ oc serviceaccounts get-token automator -n openshift-infra
```
