# OCPVirt Kafka EDA Demo

Detect OpenShift Virtualization events and trigger automation with EDA

Accompanying slide deck:

https://docs.google.com/presentation/d/1lm2ZNa2f0BwqHnoOs6Gdh6ptxUrk7jgI4pdLgiJOPac/edit#slide=id.g2f44695b139_0_0

To deploy:

1) On RHDP create a new "AWS with OpenShift Open Environment":

https://demo.redhat.com/catalog?category=Open_Environments&item=babylon-catalog-prod%2Fsandboxes-gpte.sandbox-ocp.prod

2) Wait for the environment to provision.
3) Ensure you have a ServiceNow developer account and a ServiceNow dev instance (with hostname and password)
4) Install Ansible
5) You need the following collections:

- ansible.controller (available from Red Hat Cloud Console Automation Hub only)
- ansible.eda
- redhat.openshift
- infra.controller_configuration (available from Red Hat Cloud Console Automation Hub only)
- servicenow.itsm

6) Generate an AAP Licensing Manifest File (refer to https://docs.ansible.com/automation-controller/4.4/html/userguide/import_license.html#obtain-sub-manifest for details)
7) Run the following command (substituting your relevant values):

`ansible-playbook -i <inventory_file> -e ocp_host=https://<OpenShift API Hostname>:6443 -e ocp_username=kubeadmin -e ocp_token=<token> -e snow_instance=<servicenowurl> -e snow_username="<snow_user>" -e snow_password="<servicenow password>" -e aap_manifest_path="<Local Path to AAP Manifest ZIP File for Licensing AAP>" -e demo_name=demo_virt_eda_v2 -e kafkahostname=<kafka_bootstrap> ./playbooks/_deploy_demo_on_ocp.yml`

8) Perform the remaining config for OCP Virt

This will:

1) Install the AAP Operator
2) Install AAP with default settings
3) Obtain the AAP hostname
4) Install EDA referring to the AAP host
5) Retrieve hostnames and admin passwords for both AAP and EDA
6) License AAP with the provided manifest file
7) Create the demo content within AAP
8) Debug out the hostnames and admin passwords for later access
