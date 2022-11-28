+++
author = "Geoji Paul"
categories = ["Technology", "Security"]
date = 2020-07-14T07:23:40Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1484557052118-f32bd25b45b5?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ"
slug = "so-you-want-your-kali-run-in-the-cloud"
summary = "Step by step instruction on migrating an existing Kali Linux VM to Google Cloud"
tags = ["Technology", "Security"]
title = "So you want YOUR Kali VM to run in the cloud?"

+++


So you want _your_ Kali VM to run in the cloud? Well, at least I wanted mine to run in the cloud. And If you are looking for options to migrate your _existing_ Kali VM into the cloud, these steps might be of help. The guide has details on what worked, and what did not work so that you do not have to repeat the mistakes I made.

## Background

I have been running a Kali VM in my personal laptop for the past few years. In fact, as of writing this post, the laptop is 6 years old. Thankfully, it has held up well thus far. But I know that its time is coming to an end and I will need to start migrating Kali to a new home. So was born a project out of necessity to find a new home for my VM.

After considering the option to build a home server rack, I decided against it due to space, cost, and misalignment with educational outcomes that I wanted to get out of doing the project. With a home server out of scope, I set eyes on the cloud and started looking at various options. One of the main requirements I had was that I wanted to lift and shift the current VM into the cloud.

**Stop reading if you are just wanting to spin up a _brand NEW_ Kali VM in the cloud. There are numerous how-to articles on doing it within the major cloud providers. But if you are wanting to lift-and-shift your existing VM, keep reading...**

## What Worked

All major cloud platforms offer VM import. However, since Kali is a custom OS, I could not find an out-of-the-box solution for importing Kali in any of the platforms I looked at. After exploring AWS, Azure and even containerizing my entire VM into a Docker image (I know, terrible idea...), what worked for me was Google Cloud Platform (GCP). At a very high level, migrating to GCP included creating a custom image based on the local Kali disk image and then creating a VM Instance based on the custom image. Below are the steps I followed to migrate Kali VM to GCP.

### Prepare your VM for migration into GCP

Before starting the migration process, there are a few steps that must be completed to prepare the VM. I will be using Oracle VirtualBox in the instructions below. But if you are using VM Ware, you can export your VM into a ._ovf_ format using VM Ware _ovftool_, and then import the _.ovf_ file into VirtualBox for further configuration changes.

**Enable SSH**

SSH needs to be enabled so that you can remote into your cloud hosted VM. SSH supports both password based and certificate based authentication. Below instructions are for enabling certificate based authentication.

Generate SSH Public, Private key pair using ssh-keygen

```bash
root@kali:/# cd ~/.ssh
root@kali:~/.ssh# mkdir id_rsa
root@kali:~# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): kali
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in kali.pem
Your public key has been saved in kali.pub

```

After running ssh-keygen, you will have two files. A public key (kali.pub in the above example) and a private key (kali.pem). Copy contents of the public key into the _known_hosts_ file under .ssh folder. And copy the private key (kali.pem) into your local machine from where you will remote into your cloud VM.

Next, make sure that SSH service runs on system startup

```bash
systemctl enable ssh.service

```

To make sure everything is working well, restart your VM and try to connect from your host machine to your VM using this command

```bash
ssh -i kali.pem root@IPAddressOfYourVM
```

If you a get a "Permissions are too open" error message when trying to start a connection, change the permission on the private key like this.

```bash
$ sudo chmod 600 kali.pem
```

**2. Use para-virtualized network adapter**

According to Google "_[Virtual machines with older kernel versions that do not include drivers for virtio-scsi will fail during migration](https://cloud.google.com/compute/docs/vm-migration)_". To ensure that the image import works properly, set VirtIO based network adapter.

To do so, select the Kali VM in Virtual Box, then go to _Settings >> Network >> Advanced_ and select "Paravirtualized Network (virtio-net) per the screen shot below.

{{< figure src="/post/so-you-want-your-kali-run-in-the-cloud/image.png" >}}

**3. Disable Audio**

To disable audio, select the Kali VM in Virtual Box, then go to _Settings >> Audio_ and uncheck the "Enable Audio" check box

{{< figure src="/post/so-you-want-your-kali-run-in-the-cloud/image-1.png" >}}

**4. Remove Floppy Drive from Boot Order**

To remove, Floppy Drive, select the Kali VM in Virtual Box, then go to _Settings >> System_ and uncheck the "Floppy" check box from the Boot Order

{{< figure src="/post/so-you-want-your-kali-run-in-the-cloud/image-2.png" >}}

**5. Remove swap partition**

To remove swap partition, I found this thread from StackExchange to be quite useful. Please note that this may not be a requirement for image import. If you are able to import an image with swap turned on, please write about it in the comments section.

{{< bookmark url="https://unix.stackexchange.com/questions/224156/how-to-safely-turn-off-swap-permanently-and-reclaim-the-space-on-debian-jessie" title="How to safely turn off swap permanently and reclaim the space? (on Debian Jessie)" description="I installed Debian Jessie with default partitioning on my SSD drive. My current disk partitioning looks like this: As I have 16GB of RAM, I assume I don’t need swap. But since I have other disk dr..." icon="https://cdn.sstatic.net/Sites/unix/Img/apple-touch-icon.png?v=5cf7fe716a89" author="LinuxSecurityFreak" publisher="Unix & Linux Stack Exchange" thumbnail="https://cdn.sstatic.net/Sites/unix/Img/apple-touch-icon@2.png?v=32fb07f7ce26" caption="" >}}

### Convert VirtualBox disk to raw image format

Once the VM is prepared per the steps above, shut down your Virtual Machine. The next step in the process is to create a raw disk image from the VirtualBox file format. You can either create a disk image directly using the _dd_ command or by using _qemu._ I went with the _qemu_ route since it is faster compared to _dd_ which makes a bit-to-bit copy of the disk.

**Using _qemu_**

1. Download and install qemu. [https://www.qemu.org/download/](https://www.qemu.org/download/) 

2.   Convert your virtual box disk ".vdi" format to a raw disk image

```
qemu-img convert -f vdi -O raw YourVirtualBox-disk1.vdi disk.raw
```

Where _YourVirtualBox-disk1.vdi_ is the VirtualBox virtual disk image file. You can locate it by navigating to the folder where VirtualBox stores VMs. In my case, it was under Virtual Box VMs >> 'Kali VM for GCP'

**Alternate method Using _dd_**

Run the dd command as root while logged in to your kali box

```
sudo dd if=/dev/sda of=/tmp/disk.raw bs=4M conv=sparse
```

IMPORTANT NOTE: Make sure the output file is named as **disk.raw** since GCP expects a file named "disk.raw".

### Compress the raw disk into tar.gz format

Google requires the disk image (_disk.raw)_ to be compressed using gnu-tar. If you are running OSX, make sure that _gtar_ is used for the compression as opposed to the regular _tar._

```
gtar -cSzf kali.tar.gz disk.raw
```

The disk image is now ready for upload to GCP

### Creating Custom Image in GCP

If you are new to GCP, you will need to create an account first before proceeding

**Create GCP Account**

To use GCP, you will need to have a Google cloud account and a billing account. Creating account is fairly easy. Navigate to [https://console.cloud.google.com](https://console.cloud.google.com) and follow the instructions. If you have a gmail account, it is even easier. And when you create an account, Google will automatically create a project "My First Project". If you want a different project, go ahead and create a new project where you'd like to organize your VMs. However, note that you will need to create a billing account and attach it to the project so that you can use various resources.

**Create Storage Bucket**

After creating a GCP account, create a bucket where you can upload the compressed disk image. Pick a unique name for the bucket. For location, select the single location option since the high availability options are not necessarily needed for this project. It is important to note that the storage bucket should be created in the same region as where you are going to create the custom kali image.

**Copy compressed image file to Storage Bucket**

Once the bucket is created, you can upload the tar file by either using the browser upload option, or by using Google Cloud SDK, the command line version of GCP.

If using Google Cloud SDK, use the below command to upload your file.

```
gsutil cp kali.tar.gz gs://mykali-importbucket/kali.tar.gz
```

Where _mykali-importbucket_ is the name of the bucket that was created in the previous step.

Once the upload is complete, the next step is to create a custom VM image from the raw disk image

**Creating a custom image in GCP**

Creating an image can be done using the console or through the command line. If using the console, navigate to Compute Engine >> Images >> CREATE IMAGE. Give a unique name for the image in the _Name_ field. In the _Source_ field, select "Cloud Storage file" and give the full URL to your bucket. In my case, it is _gs://mykali-importbucket/kali.tar.gz_. For location, select "Regional" and then click on create image.

Or you could use command line to accomplish the same

```
gcloud compute images create mykali --source-uri gs://mykali-importbucket/kali.tar.gz
```

Once the image is created, the final step is to create a VM Instance

**Creating a VM Instance in GCP**

For creating a VM instance using console, navigate to Compute Engine >> VM Instances >> CREATE INSTANCE. Once you are in the Create Instance page, select the appropriate system configuration (processor and memory) for your VM.

In the _Boot Disk_ section, click on "Change" to be able to select a Custom Image.

{{< figure src="/post/so-you-want-your-kali-run-in-the-cloud/image-3.png" >}}

Clicking on the "Change" button will open a _Boot Disk_ page. In the _Boot Disk_ page, select _Custom Image_ and select the image you created in previous step. Once you fill all the required fields, click on _Create_

The above steps can be done in GCP SDK using this command:

```
gcloud compute instances create mygcpkali --image mykali --machine-type n1-standard-1 --zone us-central1-a
```

If everything goes well, you will get the below notification

{{< figure src="/post/so-you-want-your-kali-run-in-the-cloud/image-4.png" >}}

### Connecting to your newly created VM

To connect to your VM, use ssh -i option

```
ssh -i kali.pem root@34.70.63.29
```

Where kali.pem is the private key that was created in the **Enable SSH** step

### References

1. Big thanks to the SecOps video that inspired me to pursue the GCP option. The video gives step by step instruction on how to do a fresh install of Kali and migrate it to GCP. [https://www.youtube.com/watch?v=25csweVVqLE](https://www.youtube.com/watch?v=25csweVVqLE)
2. GCP instuctions on importing virtual disk: [https://cloud.google.com/compute/docs/import/import-existing-image](https://cloud.google.com/compute/docs/import/import-existing-image)
3. Installing gcloud sdk in a mac book: [https://medium.com/@tapendradev/how-to-install-gcloud-sdk-on-the-macos-and-start-managing-gcp-through-cli-d14d2c3a8869](https://medium.com/@tapendradev/how-to-install-gcloud-sdk-on-the-macos-and-start-managing-gcp-through-cli-d14d2c3a8869)
4. Installing gnu-tar in mac book: [https://medium.com/@fullsour/installing-gnu-tar-on-mac-827a494b1c1](https://medium.com/@fullsour/installing-gnu-tar-on-mac-827a494b1c1)
5. Qemu convert image syntax: [https://docs.openstack.org/image-guide/convert-images.html](https://docs.openstack.org/image-guide/convert-images.html)

