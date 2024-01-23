# VM Migration from Hyper-V to OpenStack Guide

This guide outlines the process for migrating a virtual machine (VM) from Microsoft Hyper-V to an OpenStack environment.

## Prerequisites
- [Certified Guest Operating Systems in Red Hat OpenStack Platform](https://access.redhat.com/articles/973163)
- [virtio-win drivers installation guide](https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md)
- [Cloudbase cloud-init for Windows page](https://github.com/cloudbase/cloudbase-init)
- [Windows Instances Cloud Init Guide](https://github.com/gprocunier/dcib/blob/main/openstack/misc/windows-instances-cloud-init.md)
---

- Access to the Hyper-V environment and the OpenStack environment.
- Installation of `virtio-win` drivers on the Windows VM for KVM compatibility.
- Tools for VM conversion (like `qemu-img`).

## Process Overview

1. **Prep-work for the VM in Hyper-V**
2. **Export the VM Disk from Hyper-V**
3. **Convert the VM Disk to a Compatible Format**
4. **Upload the Converted Image to OpenStack**
5. **Create a New Instance in OpenStack Using the Uploaded Image**

## Detailed Steps

### Windows Team - Prep-work

- [Certified Guest Operating Systems in Red Hat OpenStack Platform](https://access.redhat.com/articles/973163)
- Note down the MAC addresses of the NICs the IP and the VLAN of the VM.
- Install drivers in the VM prior to export. Refer to the [virtio-win drivers installation guide](https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md).
- Install the cloud-init agent for Windows. Instructions can be found at the [Cloudbase cloud-init for Windows page](https://github.com/cloudbase/cloudbase-init).

### Windows Team - Steps to install drivers
### 1. Placeholder
- placeholder
- placeholder

### 2. Placeholder
- placeholder
- placeholder

### 3. Placeholder
- placeholder
- placeholder

### Windows Team - Steps to export the VM Disk
### 1. Export the VM Disk from Hyper-V
- In Hyper-V Manager, right-click the VM and select "Export."
- Choose a location to save the exported files.
    - Provide Unix team the source.vhdx for conversion
- Alternatively:
  - To export VM via PowerShell, type the command below:
```bash
Export-VM -Name \<vm name\> -Path \<path\>
```
### Unix Team - 2. Convert the VM Disk to a Compatible Format

- Use `qemu-img` or similar tool to convert the VM disk to a QCOW2 format.

  ```bash
  qemu-img convert -f vpc -O qcow2 source.vhdx destination.qcow2
  ```

### Unix Team - Upload the Converted Image to OpenStack

- Use the OpenStack CLI or Horizon dashboard to upload the QCOW2 image.

  ```bash
  openstack image create "VM_Name" --file destination.qcow2 --disk-format qcow2 --container-format bare --public
  ```

### Unix Team - Create a New Instance in OpenStack Using the Uploaded Image

- Launch a new instance in OpenStack using the uploaded image.

  ```bash
  openstack server create --image VM_Name --flavor m1.medium --network private_network VM_Instance_Name
  ```

### Additional Resources for Windows Instances and Cloud Init

- For details on Windows guest bootstrap and cloud-init, see the [Windows Instances Cloud Init Guide](https://github.com/gprocunier/dcib/blob/main/openstack/misc/windows-instances-cloud-init.md).

## Post-Migration Checks

- After migration, verify the functionality of the VM in the OpenStack environment.

## Troubleshooting

- Ensure `virtio` drivers and cloud-init are correctly installed for proper functionality.
