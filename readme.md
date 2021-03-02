# LIBVIRT WIN10 XMLs

## remind

These VM configs use a virtio SCSI drive as the base of the windows guest. This
requires the virtio-win iso to be present at first boot so that the required
drivers to detect and interface with the virtual drive can be loaded. The virtio
SCSI drivers are at `/vioscsi/w10/amd64`.

Don't forget to create the VM under `qemu:///system` instead of `qeum:///session`

May need to set hard and soft limit for memlock. The included
[limits.conf](./etc/security/limits.conf) sets it to 17GiB. The libvirt xml defs
set the maximum memory allocation to 16GiB.

[pre.xml]( ./pre.xml ) is xml for using standard SPICE based display virtualization. May be
necessary to use this before setting up GPU passthrough if windows installation
doesn't show up.

[post.xml]( ./post.xml ) is xml that only differs from pre in that it uses a
vfio isolated GPU on pcie 2c:00.0 and 2c:00.1

[perf.xml]( ./perf.xml) includes changes to increase guest
performance. Specifically, pinning 28/32 threads with the remaining 4 available
to the host having the lowest max frequency. It also specifically inclues the
"topoext" feature because apparently hyperthreading isn't available without it.

[curr.xml]( ./curr.xml) is xml that tracks my current working configuration.

**TODO** hugepages? necessary?

[grub]( ./etc/default/grub ) is an example of a grub config enabling IOMMU for AMD and
isolating the GPU by its PCI ID (one for video, one for audio).

[mkinitcpio.conf]( ./etc/mkinitcpio.conf) is a standard mkinitcpio.conf file that
requires the vfio kernel modules.
