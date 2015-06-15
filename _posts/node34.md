12 Useful “df” Commands to Check Disk Space in Linux {.blog-post-title}
----------------------------------------------------

​1. Check File System Disk Space Usage

The “**df**” command displays the information of device name, total
blocks, total disk space, used disk space, available disk space and
mount points on a file system.

    [root@tecmint ~]# df

    Filesystem           1K-blocks      Used Available Use% Mounted on
    /dev/cciss/c0d0p2     78361192  23185840  51130588  32% /
    /dev/cciss/c0d0p5     24797380  22273432   1243972  95% /home
    /dev/cciss/c0d0p3     29753588  25503792   2713984  91% /data
    /dev/cciss/c0d0p1       295561     21531    258770   8% /boot
    tmpfs                   257476         0    257476   0% /dev/shm

### 2. Display Information of all File System Disk Space Usage

The same as above, but it also displays information of dummy file
systems along with all the file system disk usage and their memory
utilization.

    [root@tecmint ~]# df -a

    Filesystem           1K-blocks      Used Available Use% Mounted on
    /dev/cciss/c0d0p2     78361192  23186116  51130312  32% /
    proc                         0         0         0   -  /proc
    sysfs                        0         0         0   -  /sys
    devpts                       0         0         0   -  /dev/pts
    /dev/cciss/c0d0p5     24797380  22273432   1243972  95% /home
    /dev/cciss/c0d0p3     29753588  25503792   2713984  91% /data
    /dev/cciss/c0d0p1       295561     21531    258770   8% /boot
    tmpfs                   257476         0    257476   0% /dev/shm
    none                         0         0         0   -  /proc/sys/fs/binfmt_misc
    sunrpc                       0         0         0   -  /var/lib/nfs/rpc_pipefs

### 3. Show Disk Space Usage in Human Readable Format

Have you noticed that above commands displays information in bytes,
which is not readable yet all, because we are in a habit of reading the
sizes in megabytes, gigabytes etc. as it makes very easy to understand
and remember.

The **df** command provides an option to display sizes in **Human
Readable** formats by using ‘-h’ (prints the results in human readable
format (e.g., 1K 2M 3G)).

    [root@tecmint ~]# df -h

    Filesystem            Size  Used Avail Use% Mounted on
    /dev/cciss/c0d0p2      75G   23G   49G  32% /
    /dev/cciss/c0d0p5      24G   22G  1.2G  95% /home
    /dev/cciss/c0d0p3      29G   25G  2.6G  91% /data
    /dev/cciss/c0d0p1     289M   22M  253M   8% /boot
    tmpfs                 252M     0  252M   0% /dev/shm

### 4. Display Information of /home File System

To see the information of only device **/home** file system in human
readable format use the following command.

    [root@tecmint ~]# df -hT /home

    Filesystem      Type    Size  Used Avail Use% Mounted on
    /dev/cciss/c0d0p5   ext3     24G   22G  1.2G  95% /home

### 5. Display Information of File System in Bytes

To display all file system information and usage
in **1024-byte** blocks, use the option ‘**-k**‘ (e.g. –block-size=1K)
as follows.

    [root@tecmint ~]# df -k

    Filesystem           1K-blocks      Used Available Use% Mounted on
    /dev/cciss/c0d0p2     78361192  23187212  51129216  32% /
    /dev/cciss/c0d0p5     24797380  22273432   1243972  95% /home
    /dev/cciss/c0d0p3     29753588  25503792   2713984  91% /data
    /dev/cciss/c0d0p1       295561     21531    258770   8% /boot
    tmpfs                   257476         0    257476   0% /dev/shm

### 6. Display Information of File System in MB

To display information of all file system usage in **MB** (**Mega
Byte**) use the option as ‘**-m**‘.

    [root@tecmint ~]# df -m

    Filesystem           1M-blocks      Used Available Use% Mounted on
    /dev/cciss/c0d0p2        76525     22644     49931  32% /
    /dev/cciss/c0d0p5        24217     21752      1215  95% /home
    /dev/cciss/c0d0p3        29057     24907      2651  91% /data
    /dev/cciss/c0d0p1          289        22       253   8% /boot
    tmpfs                      252         0       252   0% /dev/shm

### 7. Display Information of File System in GB

To display information of all file system statistics
in **GB** (**Gigabyte**) use the option as ‘**df -h**‘.

    [root@tecmint ~]# df -h

    Filesystem            Size  Used Avail Use% Mounted on
    /dev/cciss/c0d0p2      75G   23G   49G  32% /
    /dev/cciss/c0d0p5      24G   22G  1.2G  95% /home
    /dev/cciss/c0d0p3      29G   25G  2.6G  91% /data
    /dev/cciss/c0d0p1     289M   22M  253M   8% /boot
    tmpfs                 252M     0  252M   0% /dev/shm

### 8. Display File System Inodes

Using ‘**-i**‘ switch will display the information of number of used
inodes and their percentage for the file system.

    [root@tecmint ~]# df -i

    Filesystem            Inodes   IUsed   IFree IUse% Mounted on
    /dev/cciss/c0d0p2    20230848  133143 20097705    1% /
    /dev/cciss/c0d0p5    6403712  798613 5605099   13% /home
    /dev/cciss/c0d0p3    7685440 1388241 6297199   19% /data
    /dev/cciss/c0d0p1      76304      40   76264    1% /boot
    tmpfs                  64369       1   64368    1% /dev/shm

### 9. Display File System Type

If you notice all the above commands output, you will see there is no
file system type mentioned in the results. To check the file system type
of your system use the option ‘**T**‘. It will display file system type
along with other information.

    [root@tecmint ~]# df -T

    Filesystem      Type   1K-blocks  Used      Available Use% Mounted on
    /dev/cciss/c0d0p2   ext3    78361192  23188812  51127616  32%   /
    /dev/cciss/c0d0p5   ext3    24797380  22273432  1243972   95%   /home
    /dev/cciss/c0d0p3   ext3    29753588  25503792  2713984   91%   /data
    /dev/cciss/c0d0p1   ext3    295561     21531    258770    8%    /boot
    tmpfs           tmpfs   257476         0    257476    0%   /dev/shm

### 10. Include Certain File System Type

If you want to display certain file system type use the ‘**-t**‘ option.
For example, the following command will only display **ext3** file
system.

    [root@tecmint ~]# df -t ext3

    Filesystem           1K-blocks      Used Available Use% Mounted on
    /dev/cciss/c0d0p2     78361192  23190072  51126356  32% /
    /dev/cciss/c0d0p5     24797380  22273432   1243972  95% /home
    /dev/cciss/c0d0p3     29753588  25503792   2713984  91% /data
    /dev/cciss/c0d0p1       295561     21531    258770   8% /boot

### 11. Exclude Certain File System Type

If you want to display file system type that doesn’t belongs
to **ext3** type use the option as ‘**-x**‘. For example, the following
command will only display other file systems types other than**ext3**.

    [root@tecmint ~]# df -x ext3

    Filesystem           1K-blocks      Used Available Use% Mounted on
    tmpfs                   257476         0    257476   0% /dev/shm

### 12. Display Information of df Command.

Using **‘–help**‘ switch will display a list of available option that
are used with **df** command.

    [root@tecmint ~]# df --help

    Usage: df [OPTION]... [FILE]...
    Show information about the file system on which each FILE resides,
    or all file systems by default.

    Mandatory arguments to long options are mandatory for short options too.
      -a, --all             include dummy file systems
      -B, --block-size=SIZE use SIZE-byte blocks
      -h, --human-readable  print sizes in human readable format (e.g., 1K 234M 2G)
      -H, --si              likewise, but use powers of 1000 not 1024
      -i, --inodes          list inode information instead of block usage
      -k                    like --block-size=1K
      -l, --local           limit listing to local file systems
          --no-sync         do not invoke sync before getting usage info (default)
      -P, --portability     use the POSIX output format
          --sync            invoke sync before getting usage info
      -t, --type=TYPE       limit listing to file systems of type TYPE
      -T, --print-type      print file system type
      -x, --exclude-type=TYPE   limit listing to file systems not of type TYPE
      -v                    (ignored)
          --help     display this help and exit
          --version  output version information and exit

    SIZE may be (or may be an integer optionally followed by) one of following:
    kB 1000, K 1024, MB 1000*1000, M 1024*1024, and so on for G, T, P, E, Z, Y.

    Report bugs to <bug-coreutils@gnu.org>.

**Source: [http://www.tecmint.com/how-to-check-disk-space-in-linux/](http://www.tecmint.com/how-to-check-disk-space-in-linux/)**

Enviado por jguzman el Sáb, 08/23/2014 - 05:19

-   [blog de
    jguzman](/es/blog/1 "Leer últimas entradas al blog de jguzman.")

