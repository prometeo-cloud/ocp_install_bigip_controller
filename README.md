# ocp_install_bigip_controller

Installs the F5 BIG-IP Controller pod on OCP.

The F5 BIG-IP Controller manages the F5 BIG-IP device from OpenShift using environment’s native CLI/API.

### Example of use

Create a file site.yml as follows:

```yaml
- hosts: localhost
  tasks: 
    - name: "install bigip controller"
      include_role: 
        name: ocp_install_bigip_controller
      vars:
        ocp_token: "<<service_account_token>>"
        bigip_username: "username_here"
        bigip_password: "password_here"
        bigip_url: "bigip_admin_ip"
        bigip_partition: "partition_name"
        route_vserver_addr: "virtual_server_bind_address"
        log_level: "INFO"
        openshift_sdn_name: "vxlan_tunnel_name"
        default_server_ssl: "server_ssl_name"
        route_label: "ocp_route_filter_label"
```
Deploy this role in a roles folder where the above site.yml file is.

Execute as follows:

```bash
$ ansible-playbook site.yml
```  
### Configuration variables

| name | description | required | example |
|---|---|---|---|
| ocp_token | A service account token with admin rights on the kube-system namespace. | yes | see below secrion how to create a token...
| bigip_username| BIG-IP iControl REST username. | yes | |
| bigip_password | BIG-IP iControl REST password. | yes | |
| bigip_url | BIG-IP admin IP address. | yes | |
| bigip_partition | The BIG-IP partition in which to configure objects. | yes |
| route_vserver_addr | Bind address for virtual server for OpenShift Route objects. | no |
| log_level | Log level. Defaults to INFO. Possible values are INFO, DEBUG, CRITICAL, WARNING, ERROR. | no | DEBUG |
| openshift_sdn_name | Name of the VXLAN tunnel on the BIG-IP system that corresponds to an Openshift SDN HostSubnet. | no | |
| default_server_ssl | Specify the name of a user created server ssl profile that will be attached to the route https vserver and used as default for SNI. This profile must have the Default for SNI field enabled. | no |
| route_label | Tells the k8s-bigip-ctlr to only watch for OpenShift Route objects with the f5type label set to this value. | no | |
          
For more information on these variables see [here](https://clouddocs.f5.com/products/connectors/k8s-bigip-ctlr/latest).

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
