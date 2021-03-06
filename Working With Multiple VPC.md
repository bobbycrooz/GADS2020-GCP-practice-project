project Title: Working with multiple VPC





 #Task 1. Create custom mode VPC networks with firewall rules
----------------------------------------------------
Create two custom networks, managementnet and privatenet, along with firewall rules to allow SSH, ICMP, and RDP ingress traffic.
##1. Create the managementnet network

    >gcloud compute networks create managementnet --project=qwiklabs-gcp-01-e83552426f75 --subnet-mode=custom --bgp-routing-mode=regional

    >gcloud compute networks subnets create managementsubnet-us --project=qwiklabs-gcp-01-e83552426f75 --range=10.130.0.0/20 --network=managementnet --region=us-central1


##2. create a privatenet vpc
to create privatenet network

    > gcloud compute networks create privatenet --subnet-mode=custom

##3. privatenet us subnet
    >gcloud compute networks subnets create privatesubnet--us --network=privatenet --region=us-central1 --range=172.16.0.0/24

##4. privatnet eu subnet
    >gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-west1 --range=172.20.0.0/20

#Task 2. to check the list of networks

    >gcloud compute networks list

##5. list sorted by network

    >gcloud compute networks subnets list --sort-by=NETWORK
------------------------------------------------

##1. creating firewwall rule for managmentnet

    >gcloud compute --project=qwiklabs-gcp-01-e83552426f75 firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=tcp:22,tcp
:3389,icmp --source-ranges=0.0.0.0/0


##2. Create the firewall rules for privatenet
    >gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0


##3. creating virtual machine
    >gcloud beta compute --project=qwiklabs-gcp-01-e83552426f75 instances create managementnet-us-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=64937332247-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=managementnet-us-vm --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any


##4. create virtual machine for privatenet-us-vm
    >gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=privatesubnet-us

##5. list all vm instance and soorted by zone
    >gcloud compute instances list --sort-by=ZONE


##6. vm with multiple network interface
    >gcloud beta compute --project=qwiklabs-gcp-01-e83552426f75 instances create vm-appliance --zone=us-central1-c --machine-type=n1-standard-4 --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=64937332247-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-appliance --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

CONCLUSION:
they all can connect with there internal iP adresses except for munet-eu-vm

