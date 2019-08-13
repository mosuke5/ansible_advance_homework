# Advanced Deployment with Ansible Homework Assignment
This is the README for Advanced Microservices Development Homework Assignment.  
Repository is https://github.com/mosuke5/ansible_advance_homework 

## How to use
### Configure Ansible Tower
You can use `site-config-tower.yml` to provision configurations of Ansible Tower. This playbook makes some configuration like Job template, Workflow template, credentials and so on. Before using it, you need to configure.

```
# On bastion server
$ export TOWER_GUID=xxxx
$ export OSP_GUID=xxxx
$ export OPENTLC_LOGIN=xxxx
$ export OPENTLC_PASSWORD=xxxx
$ export GITHUB_REPO=https://github.com/mosuke5/ansible_advance_homework
$ export JQ_REPO_BASE=http://www.opentlc.com/download/ansible_bootcamp
$ export REGION=us-east-1
$ export RH_MAIL_ID=xxxx
$ ansible-playbook site-config-tower.yml -e tower_GUID=${TOWER_GUID} -e osp_GUID=${OSP_GUID} -e opentlc_login=${OPENTLC_LOGIN} -e path_to_opentlc_key=/root/.ssh/mykey.pem -e param_repo_base=${JQ_REPO_BASE} -e opentlc_password=${OPENTLC_PASSWORD} -e REGION_NAME=${REGION} -e EMAIL=${RH_MAIL_ID} -e github_repo=${GITHUB_REPO}
```

### Exec workflow
After provisioning Tower configuration, you can exec CI/CD workflow template. This workflow contains following.

1. Provision production environment(AWS)
2. Provision QA environment(OpenStack)
3. Deploy 3tier application to QA environment
4. Test application on QA environment
    - If test fail, QA environment will be deleted, and this workflow will end.
5. Deploy 3tier application to production environment
6. Test application on production environment

You can exec this workflow in following way.
1. Log in to Ansible Tower
2. Click "Template" link on the left side
3. Find "cicd_workflow" template
4. Click its rocket button on the right

### Grading
If your cicd_workflow is executed sucucessfully, you also can self-grade it. `grading-script.yml` is the self-grading playbook.

```
$ OSP_GUID=xxxx
$ ANSIBLE_ADVANCE_GUID=xxxx
$ ansible-playbook grading-script.yml -e OSP_GUID=${OSP_GUID} -e ANSIBLE_ADVANCE_GUID=${ANSIBLE_ADVANCE_GUID}
```

Following is sample output of self-grading.

```
PLAY [localhost] *********************************************************************************************************************************************

TASK [Create In-memory Inventory] ****************************************************************************************************************************
changed: [localhost]

PLAY [workstation] *******************************************************************************************************************************************

TASK [OpenStack servers] *************************************************************************************************************************************
ok: [workstation-43e0.rhpds.opentlc.com]

TASK [Curl website] ******************************************************************************************************************************************
changed: [workstation-43e0.rhpds.opentlc.com] => (item={u'vm_state': u'active', u'OS-EXT-STS:task_state': None, u'addresses': {u'int_network': [{u'OS-EXT-IPS-MAC:mac_addr': u'fa:16:3e:59:c9:f2', u'version': 4, u'addr': u'20.20.20.5', u'OS-EXT-IPS:type': u'fixed'}, {u'OS-EXT-IPS-MAC:mac_addr': u'fa:16:3e:59:c9:f2', u'version': 4, u'addr': u'10.10.10.3', u'OS-EXT-IPS:type': u'floating'}]}, u'image': {u'id': u'dec75175-af8b-41e2-9ef6-626b53a24ea5'}, u'OS-EXT-SRV-ATTR:user_data': None, u'OS-EXT-SRV-ATTR:root_device_name': None, u'OS-SRV-USG:launched_at': u'2019-08-13T06:25:15.000000', u'flavor': {u'id': u'bf05c36a-205e-415e-b810-51e3e9d56357'}, u'reservation_id': None, u'networks': {}, u'cloud': u'ospcloud', u'scheduler_hints': None, u'user_id': u'97656d07875c40b38805582946989e49', u'location': {u'project': {u'domain_name': None, u'domain_id': None, u'name': u'admin', u'id': u'1e3aa106a2f94e93bb328f6bebe8b965'}, u'cloud': u'ospcloud', u'region_name': u'RegionOne', u'zone': u'nova'}, u'power_state': 1, u'interface_ip': u'10.10.10.3', u'instance_name': u'instance-00000015', u'personality': None, u'OS-EXT-SRV-ATTR:ramdisk_id': None, u'updated': u'2019-08-13T06:25:14Z', u'OS-SRV-USG:terminated_at': None, u'OS-EXT-SRV-ATTR:reservation_id': None, u'key_name': u'ansible_ssh', u'disk_config': u'MANUAL', u'host_id': u'b3aa7068069c188c8861a9add78f3de5e78d5181e211b8018415dd80', u'project_id': u'1e3aa106a2f94e93bb328f6bebe8b965', u'locked': None, u'name': u'frontend', u'adminPass': None, u'launch_index': None, u'host_status': None, u'config_drive': u'', u'created_at': u'2019-08-13T06:24:57Z', u'server_groups': None, u'terminated_at': None, u'ramdisk_id': None, u'user_data': None, u'OS-EXT-STS:vm_state': u'active', u'OS-EXT-SRV-ATTR:instance_name': u'instance-00000015', u'az': u'nova', u'id': u'78309442-5c02-463f-b0e2-09c5f80f6081', u'security_groups': [{u'name': u'frontend'}], u'os-extended-volumes:volumes_attached': [], u'OS-EXT-SRV-ATTR:hostname': None, u'hostname': None, u'OS-DCF:diskConfig': u'MANUAL', u'accessIPv4': u'10.10.10.3', u'accessIPv6': u'', u'OS-SCH-HNT:scheduler_hints': None, u'progress': 0, u'OS-EXT-STS:power_state': 1, u'OS-EXT-AZ:availability_zone': u'nova', u'launched_at': u'2019-08-13T06:25:15.000000', u'metadata': {u'group': u'frontends', u'deployment_name': u'QA'}, u'status': u'ACTIVE', u'hostId': u'b3aa7068069c188c8861a9add78f3de5e78d5181e211b8018415dd80', u'OS-EXT-SRV-ATTR:host': u'comp01.example.com', u'description': None, u'tags': [], u'kernel_id': None, u'public_v4': u'10.10.10.3', u'block_device_mapping': None, u'public_v6': u'', u'hypervisor_hostname': u'comp01.example.com', u'private_v4': u'20.20.20.5', u'host': u'comp01.example.com', u'OS-EXT-SRV-ATTR:kernel_id': None, u'task_state': None, u'properties': {u'OS-EXT-SRV-ATTR:ramdisk_id': None, u'OS-EXT-STS:task_state': None, u'OS-EXT-SRV-ATTR:host': u'comp01.example.com', u'OS-SRV-USG:terminated_at': None, u'OS-EXT-SRV-ATTR:reservation_id': None, u'OS-EXT-SRV-ATTR:user_data': None, u'OS-EXT-STS:vm_state': u'active', u'OS-EXT-SRV-ATTR:instance_name': u'instance-00000015', u'OS-EXT-SRV-ATTR:root_device_name': None, u'OS-SRV-USG:launched_at': u'2019-08-13T06:25:15.000000', u'OS-EXT-SRV-ATTR:hypervisor_hostname': u'comp01.example.com', u'OS-EXT-SRV-ATTR:kernel_id': None, u'host_status': None, u'locked': None, u'OS-EXT-SRV-ATTR:launch_index': None, u'OS-EXT-SRV-ATTR:hostname': None, u'OS-DCF:diskConfig': u'MANUAL', u'os-extended-volumes:volumes_attached': [], u'trusted_image_certificates': None, u'OS-SCH-HNT:scheduler_hints': None, u'OS-EXT-STS:power_state': 1, u'OS-EXT-AZ:availability_zone': u'nova'}, u'OS-EXT-SRV-ATTR:hypervisor_hostname': u'comp01.example.com', u'OS-EXT-SRV-ATTR:launch_index': None, u'created': u'2019-08-13T06:24:57Z', u'tenant_id': u'1e3aa106a2f94e93bb328f6bebe8b965', u'region': u'RegionOne', u'has_config_drive': False, u'trusted_image_certificates': None, u'root_device_name': None, u'volumes': []})
 [WARNING]: Consider using the get_url or uri module rather than running 'curl'.  If you need to use command because get_url or uri is insufficient you can
add 'warn: false' to this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.


TASK [Fail] **************************************************************************************************************************************************
skipping: [workstation-43e0.rhpds.opentlc.com]

PLAY [localhost] *********************************************************************************************************************************************

TASK [Curl website] ******************************************************************************************************************************************
changed: [localhost]

TASK [debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "webpage.stdout": "<!doctype html>\n\n<html lang=\"en\">\n<head>\n  <meta charset=\"utf-8\">\n\n  <title>Ansible Deployed Tomcat</title>\n  <meta name=\"description\" content=\"Ansible Created Content\">\n  <meta name=\"tony.g.kay@gmail.com\" content=\"Ansible\"SitePoint\">\n\n</head>\n\n<body>\n    <h1>Ansible has done its job - Welcome to Tomcat on 3.224.182.107</h1>\n</body>\n</html>"
}

TASK [Fail] **************************************************************************************************************************************************
skipping: [localhost]

TASK [Success] ***********************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Congrats Yours HW Assignment is completed"
}

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=4    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
workstation-43e0.rhpds.opentlc.com : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```