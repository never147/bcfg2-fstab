bcfg2-fstab
===========

Options
-------
Check the options in the header of the template at Cfg/etc/fstab/fstab.genshi

Default Filesystem to use if it can't be detected

    Default_Filesystem = 'ext3'

Default mount options for all file systems

    Default_Mount_Opts = 'noatime'

Extra mount options for ext3

    Extra_Ext3_Opts = ''

Extra mount options for ext4

    Extra_Ext4_Opts = 'nodelalloc'

Extra mount options for XFS

    Extra_XFS_Opts = 'nobarrier'

Default device to use for root if it cannot be detected

    Default_root_device = '/dev/sda'

Default device to use for swap if it cannot be detected

    Default_swap_device = '/dev/sdb'

List all the names of the probes here (without the leading _ and -uuid)

    Disk_Probes = ['swap','root','usr','var','boot','usr_local']

Properties file
---------------
Mount points in /etc/fstab can be listed in the Properties/mounts.xml file.
Field names are taken from the fstab manual. There are no defaults so all fields are needed.
device mount_point filesystem options dump fsck

    <Mount spec='tmpfs' file='/dev/shm' vfstype='tmpfs' mntopts='defaults' freq='0' passno='0'/>

The root and swap partitions are assumed to exist on all hosts and if not present or detected forced.
Check default options in template for the assumed device names. Use probes for UUID to ensure they get
written back.

Probes
------
All partitions that exist on servers should have a _partion_uuid probe.
The results of the probe should be the UUID of the partition.
Underscores in the probe name are converted to slashes '/' in the template.
Some examples are given in the probes directory.

The root filesystem probe _root_fs detects what filesystem format is in use. It's then assumed for
all the other probes. If you have a partition with a different format put it in Properties/mounts.xml
Note the probed uuid mount point will still exist if it's a common name. Having a different fs type
to a commonly used fs is not supported (yet).

User specified mounts
---------------------
A probe named _mount_config will look for a begin/end section in the servers fstab
which will be persisted in the template and written back to the server

    #BEGIN_MOUNT_CONFIGURATION
    /dev/foo /foo crazyfs defaults 0 0
    #END_MOUNT_CONFIGURATION

Matt Baker (2012)
