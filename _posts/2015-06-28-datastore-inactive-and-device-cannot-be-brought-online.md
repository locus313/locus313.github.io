---
title: Datastore inactive and device cannot be brought online
layout: post
categories: [VMWare]
---
If your shared storage or volumes go offline while VMs are running, but after recover these volumes and rescan adapter,
datastores still stay in inactive state and you're seeing the following in the /var/log/vmkernel.log:

`ScsiDevice: 5192: eui.3f7999dba06450376c9ce9006ec1b4eb device :Open count > 0, cannot be brought online`

it may indicate that the virtual machine is stuck or specifically the world (process) for the virtual machine vCPU is still holding 
up to the device. Since the datastore and the backed storage device/volume were not unmounted and detached properly, it could not be
brought online again after recovery. At least not until we can kill the stuck VM. Here's the step to do that:

1. Identify the inactive datastore and the device serial number behind it (which would be similar to the one shown in the vmkernel.log)
2. Kill all related world (by id) to the device on the ESX host. Here's a sample script:
```bash
DEVICE_SERIAL=eui.3f7999dba06450376c9ce9006ec1b4eb
for i in $(esxcli storage core device world list -d $DEVICE_SERIAL |awk {'print $2'} |tail -n +3)
do
out=$(esxcli vm process kill --type=force --world-id=$i)
rc=$?
if [[ $rc -eq 0 ]]; then
echo "kill world id=$i successfully"
fi
# check for error
echo $out |grep "Unable to find a virtual machine with the world ID" 1>/dev/null
rc=$? # rc=0 means world id not found which is OK
if [[ $rc -eq 1 ]]; then
echo ERROR: "$out"
fi
done
```

3. Rescan adapter and the device/datastore would come back to normal after it completes

Source:
 <http://importstack.blogspot.com/2014/10/datastore-inactive-and-device-cannot-be.html>