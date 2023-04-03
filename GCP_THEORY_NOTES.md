## Day - 1

* Google Compute Engine Service is the first google service.
* Google compute engine is like a virtual machine on cloud.
* Google compute engine is also knows as GCE.
* Load balancing and auto-scaling.
* hostname -i to get internal IP address.
* There are various family of GCE like e2, c3, etc
* e2 is basic family with min 2-vcpu and 4GB-RAM.
* Default OS is Debian with 10GB OS-Disk.
* Each GCE has one Internal IP and One External IP.
* We can skip the External IP address but Internal is compulsory
* External IP address can be lost after the VM restart.
* We can use a static external IP.
* To use static external IP follow below steps
	- Go VPC Service.
	- Select the External IP address visible on the screen and click on "RESERVE EXTERNAL STATIC ADDRESS."
* We can switch the Static IP between GCE if they both are in same Google Project.(example PROJECT-Rushikesh)
* We have to manually detach and assign to another GCE.
* If we are using a static IP we are charged extra even if its not in use.
* If we delete the GCE with static IP assigned to it, then the static IP is not deleted we have to manually delete it.
* Note:- Release the STATIC IP if not in use.
* Start-Up script:- If we want to perform number of sequential steps after the OS installation we can use strart up script under Management section while creating GCE.

## Day - 2
* We can create instance using the **Instance-Template.**
* **Instance-Template** is the process of creating a tempate of all the basic information to create for VM like, selecting GCE family e2 or c3, OS type and OS-Disk space, RAM space, start-up script.
```sh
#!bin/bash
apt-get update
apt-get -y install apache2
echo "Hello message $(hostname) $(hostname -i)" > /var/www/html/index.html
```
* Above is the example of a start-up script.
* For creating a **Instance-Template** we are not charged.
* For creating VM from **Instance-Template** we are charged.
* Once we create a VM from **Instance-Template** we can delete that **Instance-Template.**
* If we want to reduce the start-up time while creating the VM then we can use a**CUSTOM-IMAGE.**
* **CUSTOME-IMAGE** is a copy/clone of OS-disk that is attached to other VM.
* Note:- While creating a **CUSTOM-IMAGE** from another VM, the VM shoule be in **STOP** status.
* Once a "CUSTOM-IMAGE" is created we can create VM from it directly or by creating **Instane-Template** first and then VM from newly created **Instance-Template.**
* If we are not using the CUSTOM-IMAGE any more then we can delete the Image or marke it as **DEPRECATED.**

## Day - 3
* **Sustained use Discount** is a type of discount that we get if we use specific type of VM for significant period of the month.
* If we use N1 and N2 type VM for more then 25% period of the month then we can get around 20%-50% discount on every incremental period.
* Example:- In a month if we use N1-N2 type of VM for more then 10950 min then we get 20-50% discount of the above period.
* Note:- This discount is valid if we create VM using Kubernetes Engine and Compute Engine. This discount is not applicable for E2 and A2 type of VM. Also VM created by App Engine and Dataflow this discount is **not applicable.**
* We do not need to apply for this discount it is applied directly.
* **Committed use Discount** is a type of discount that we get if we use specific type of VM for prolonged duration(Years).
* If we are going to use VM for prolong duration then we can get upto 70% discount based on VM type and GPU.
* **Committed use Discount** is goof if we are using the VM for long term(Years).
* Same ristriction as of **Sustained use Discount.**
* We need to apply for this discount while creating the VM.
* We need to create a commitment and use same commitment while creating other VM.
* **Committed use Discount** is greter than **Sustained use Discount**.
* **Sustained use Discount** is good if we are using the VM for short term(Monthly).
* **Preemptible VM** are short-lived VM and very cheap as compared to normal VM.
* Max duration is 24 hour, but GCP can stop them any time during this period with notice of 30 seconds.
* Do not come under Free-Tier.
* We can not convert this VM into normal VM.
* **Spot VM** are the latest version of this VM.
* Only main difference is that their is now max duration limit now.
* Google Compute Engines are billed by the seconds(after minimum of 1min).
* If VM is in stop state then we are not billed.
* But if any storage is attached to stop VM then we will be billed on storage.
* If Host machine on which VM is running needs to be updated(Hardware/Software) then all VMs present on the machine will be migrated to new host machine in same zone. This is called as Live Migration.
* By default when we create VM **On Host Maintenance** is set to **Live migration**, we can also set it to **Terminate VM**.
* **Auto-Restart** if any VM is terminated due to some system issue/ hardware issue or non-user-interaction issue then we can Auto-Restart the VM.
* Both this **Live migration** and **Auto-Restart** needs to be set while creating VM under **Advance Option->Management->Availability Policies**.
* We can create Custom VM type if for some reason existing VM type is not sufficient for workload.
* First we need to select VM family then we can select custom type(VCPU, Memory, GPU)
* Lastly select the OS type.
* Billing formula is as follow.
	- $0.033174/vCPU(per) + $0.004446/Memory-GB(per) this is for hourly.
* We can create Custom VM on create VM page under Machine **Configuration->Machine Type->Custom Type**.
* GPU are used when worload is related to high computaion and graphic intensive(AI/ML).
* GPU is attached to VM while creating a new VM.
* GPU are expensive.
* We need to install GPU libraries on VM so that we can use GPU.
* Not all VM series/Family are supported.
* Auto-Restart must be enabled.
* We can not change the VM Type if VM is in Running State.
* Note:- VM are zone specific and project sepefic, image are global and can be used in other project, Instance Template is also global(If we not using any zone specific resource.)
* Automactic Basic motinoring like (CPU utilization, Network Bytes, Disk Throughput)is enabled by default.
* If we want dedcated Host machine for VMs then first we need to create **Sole tenant node** with **Affinity label**.
* While creating VM we need to use same **Affinity lable** to create VM in our dedicated Host machine.
* If we want to apply some OS-patch-management to large number of VMs we can use **VM-Manager**.


### Day - 4
#### GCloud
* Using GCP Gcloud SDK to automate resources.
* Documentation for gcloud commands is https://cloud.google.com/sdk/gcloud/reference.
* We can download SDK on OS or we can use it in google cloud shell.
* We need to configure(Authorize Account, Select Account, Project, Region and Zone) the gcloud before using.
* We can do this using the below command
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud init
Welcome! This command will take you through the configuration of gcloud.

Pick configuration to use:
 [1] Re-initialize this configuration [my-default-configuration] with new settings
 [2] Create a new configuration
 [3] Switch to and re-initialize existing configuration: [cloudshell-14523]
Please enter your numeric choice:  
```
* We can create multiple configuration for gcloud.
* Command to create new configruation is as below.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud config configuration create {configuration_name}
```
* The create command not only create new configuration but also activate it.
* To activate any other configration we need to use activate command.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud config configuration activate {configuration_name}
```
* To view current configuration we can use below command.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud config list
[accessibility]
screen_reader = True
[component_manager]
disable_update_check = True
[compute]
gce_metadata_read_timeout_sec = 30
region = us-central1
zone = us-central1-a
[core]
account = rushikesh.bhavsar.10@gmail.com
disable_usage_reporting = True
project = project-rushikesh
[metrics]
environment = devshell

Your active configuration is: [my-default-configuration]

```
* The above command will list all the configuration for the gcloud.
* If we want to see the **region** then command will be as follows.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud config list compute/region
[compute]
region = us-central1

Your active configuration is: [my-default-configuration]
```
* By default when we use **list {property name}** config then **core** section property will be shown.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud config list account
[core]
account = rushikesh.bhavsar.10@gmail.com

Your active configuration is: [my-default-configuration]
```
* We can change the value of any property using the **SET** command.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud config set compute/zone us-central1-a

Updated property [compute/zone].
```
* Command structure **"gcloud group sub-group action"** for accessing resources using gcloud.
* Group can be resources type like Compute Engine, Network and many more.
* Sub-Group will be sub-resources like Instance, OS-Disk, Instance Template and many more.
* To create a VM instance using gcloud is **gcloud compute instance create {Instance_name}**.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud compute instances create first-instance-using-gcloud

Created [https://www.googleapis.com/compute/v1/projects/project-rushikesh/zones/us-central1-a/instances/first-instance-using-gcloud].
NAME: first-instance-using-gcloud
ZONE: us-central1-a
MACHINE_TYPE: n1-standard-1
PREEMPTIBLE:
INTERNAL_IP: 10.128.0.11
EXTERNAL_IP: 34.172.255.202
STATUS: RUNNING
```
* We can also use delete this instance using the delete command.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud compute instances delete first-instance-using-gcloud
The following instances will be deleted. Any attached disks configured to be auto-deleted will be deleted unless they are attached to any other instances or the `--keep-disks` flag is given
and specifies them for keeping. Deleting a disk is irreversible and any data on the disk will be lost.
 - [first-instance-using-gcloud] in [us-central1-a]

Do you want to continue (Y/n)?  Y

Deleted [https://www.googleapis.com/compute/v1/projects/project-rushikesh/zones/us-central1-a/instances/first-instance-using-gcloud].
```
* We can also use filter with list command.
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud compute machine-types list --filter zone:asia-southeast2-b
```
* We can pass multiple value to filter using ()
```sh
rushikesh_bhavsar_10@cloudshell:~ (project-rushikesh)$ gcloud compute machine-types list --filter "zone:(asia-southeast2-b asia-southeast2-c)"
```
* Below are some command related to compute engine.
```sh
gcloud compute instances list
gcloud compute instances create {instance_name}
gcloud compute instances describe {instance_name}
gcloud compute instances delete {instance_name}
gcloud compute zones list
gcloud compute regions list
gcloud compute machine-types list
gcloud compute zones list --sort-by=region
gcloud compute zones list --sort-by=~region
gcloud compute zones list --uri
```
* Before creating the any instance need to specify the COMPUTE_ENGINE_REGION and COMPUTE_ENGINE_ZONE.
* We can set region and zone in three ways
	1. Central Configuration using gcloud compute project-info add-metadata [values]
	2. Using gcloud init or gcloud config set compute/{property_name} = value
	3. Using --zone and --region flag with compute command.
* Priority is 3>2>1.

#### Instance Group
* Multiple VM together are called as **Instance Group**.
	1. Managed Instance group.
		- VMs created using same template.
		- Auto-scaling, Auto-Healing and many more services are available.
		- We can not stop the instance from the group.
	2. Unmanaged Instance group.
		- VMs which are not identical.
		- Auto-scaling, Auto-Healing and many more services are not available.
* Location of VM should be in different zone of same region.
* Auto-Scaling can be enabled based on multiple matrics.
	- CPU utilization.
	- Load Balancer.
	- Cloud Monitoring metric.
* Cool down period:- How long to wait before looking at auto-scaling matric again.
* Scale-in control:- Sudden drop in no of VM. If eco-system is stable then we do not want to scale down immediately. Scale down by small number of VMs.
* Auto Healing is the process of checking the health of VM and if the health of one VM is down then new VM is added.
* While creating the instance group all zone of the perticular region should support of **machine type**.
* While creating the instance group health-check creation is must.
* Predictive scaling is process of studing the previous auto-scaling data and then perform auto-scaling.
* We can edit the Instance group after creation of the group and changes will reflect.
##### Update Managed Instance Group
* There are two methods for Update of MIG.
	1. Rolling Update.
	2. Rolling Restart/Replace.
* **Rolling Update** - Update of VMs with new templates in small groups at a time(2/3).
* **Canary testing** - Update 2/3 VMs with new instance template test it and then roll out for remaining instances.
* There are two options on when the update should take place.
	1. Proactive - Start the Update immediately. Ideally Proactive is used.
	2. Opportunistic - Start the update later when we resize instance group i.e restart of the VMs.
* Maximum Surge: How many instances can be added before deleting the old instances. This is used in Replace option only.
* Maximum unavailable: How many instances can be unavailable during Restart and Replace option.
* Example: Out of 10, 8 will serve request and 2 will be offline.
* Rolling Update can be under **Instance Groups -> Select instance-group -> Update VMS**.
* **Rolling Restart/Replace** - **No Update in the instance template**, just restart the existing VMs or create new VMs from the existing **Instance template**.
* Maximum Surge, Maximum unavailable and what we want to perform(Restart/Replace) is same as Rolling Update.
* Rolling Restart/Replace can be under **Instance Groups -> Select instance-group -> Restart/Replace VMS**.
