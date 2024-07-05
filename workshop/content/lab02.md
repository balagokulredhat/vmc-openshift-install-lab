Now we're going to run some ansible to prepare our lab environment to install OpenShift using UPI.

### SSH into your bastion
We're going to be running this lab from your bastion in VMC, so start by SSHing into it with the hostname and username/password you got from RHDP:
```execute
ssh lab-user@bastion.%GUID%.dynamic.opentlc.com
```

### Clone this repository and cd into it
```execute
git clone https://github.com/naps-product-sa/vmc-openshift-install-lab.git
cd vmc-openshift-install-lab
```

### Install ansible
```execute
sudo python3 -m pip install ansible
```

### Update your vars file
Next let's add some properties to `ansible/vars.yml`. We're interested in this section:
```bash
###### YOUR PROPERTIES HERE ######
guid: 'CHANGEME'
vcenter_password: 'CHANGEME'
ocp4_pull_secret: 'CHANGEME'
##################################
####### OCP PROPERTIES HERE ######
ocp_domain: 'CHANGEME'
ocp_version: '4.13.15' #Default is ocp 4.13.15, if requiredyou can change this
venter_datastore: 'CHANGEME'
##################################
```
1. Replace the existing values for the guid (%GUID%) and vCenter password you got from RHDP.
2. Grab your OpenShift Pull Secret from the [Red Hat Hybrid Cloud Console](https://console.redhat.com/openshift/install/vsphere/agent-based) and paste it *between the single ticks* for the `ocp4_pull_secret` variable.
3. On the latest update on 28th Jun 2024 the vcenter domain and the config has been updated, since then you have to pass the venter_datastore you can get the name from the vcenter datastore (Enter any one of the datastore that you can see under the datastore). 

### Wait, what am I about to run?
The playbook we're about to run has two plays: `Configure bastion for OCP4 UPI` and `Create OpenShift Installation Directory and Prereqs`. Let's take a closer look at each of them for a moment.
