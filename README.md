bcfg2-fstab
===========

Check the options in the header of the template at Cfg/etc/fstab/fstab.genshi

Mount points in /etc/fstab can be listed in the Properties/mounts.xml file.
Field names are taken from the fstab manual. There are no defaults so all fields are needed.
device mount_point filesystem options dump fsck

    <Mount spec='tmpfs' file='/dev/shm' vfstype='tmpfs' mntopts='defaults' freq='0' passno='0'/>

The root and swap partitions are assumed to exist on all hosts and if not present or detected forced.
Check default options in template for the assumed device names. Use probes for UUID to ensure they get
written back.

All partitions that exist on servers should have a _partion_uuid probe.
The results of the probe should be the UUID of the partition.
Underscores in the probe name are converted to slashes '/' in the template.
Some examples are given in the probes directory.

The root filesystem probe _root_fs detects what filesystem format is in use. It's then assumed for
all the other probes. If you have a partition with a different format put it in Properties/mounts.xml
Note the probed uuid mount point will still exist if it's a common name. Having a different fs type
to a commonly used fs is not supported (yet).

A probe named _mount_config will look for a begin/end section in the servers fstab
which will be persisted in the template and written back to the server

    #BEGIN_MOUNT_CONFIGURATION
    /dev/foo /foo crazyfs defaults 0 0
    #END_MOUNT_CONFIGURATION

Matt Baker (2012)
