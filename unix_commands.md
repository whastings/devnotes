# Unix Commands
**Various commands for system and third-party Unix/Linux programs.**

* `ab -n 100 -c 50 -k -H 'Accept-Encoding: gzip,deflate' url`
  * Test the connectivity of a host (requires Apache)
* `adduser user group`
  * Add an existing user to a group
* `alias`
  * Show currently set command aliases
* `command arg1 arg2 &`
  * Send a command to the background by postfixing it with an ampersand
* `sudo /etc/init.d/apache2 restart`
  * Restart apache
* `apt-cache show packageNameHere`
  * Show info about an installed package
* `sudo apt-get update`
  * Update repositories list and find any software upgrades
* `sudo apt-get upgrade`
  * Update packages with updates available
* `aspell -c filename`
  * Check a file for spelling errors.
* `bg`
  * Send current foreground process to the background. Press control + z first to suspend the running process
* `cat file1.txt file2.txt > file3.txt`
  * Reads file1.txt and file2.txt and combines those files to make file3.txt.
* `cat id_rsa.pub >> authorized_keys`
  * Appends file on left to file on right
* `ccsm`
  * Start Compiz config manager
* `chgrp group file`
  * Change group of file or directory
* `sudo chmod o+x /home`
  * all executing for a directory, which is necessary for access by Apache
* `sudo find /path/to/someDirectory -type d -print0 | xargs -0 sudo chmod 755`
  * Set all directories under a specific directory to 755
* `sudo find /path/to/someDirectory -type f -print0 | xargs -0 sudo chmod 644`
  * Set all files under a specific directory to 644
* `crontab -e`
  * edit user's crontab file
* `crontab -l`
  * view user's crontab file
* `crontab -u user -l`
  * View another user's crontab file
* `sudo crontab -l`
  * View root's cron file
* `curl -C url`
  * Resume a previous download
* `curl -H "header: value" url`
  * Request URL with a custom header
* `curl -I host`
  * See request data for a host
* `curl -i url`
  * Include response headers in the output
* `curl url -O`
  * Save download using name from URL
* `curl url -o filename`
  * Save download to filename
* `date --date="string"`
  * Convert some data string to a standardized date string. Can be specific date or relative (e.g. yesterday)
* `date --date=@timestamp`
  * Convert unix timestamp to date
* `date -u`
  * Display date/time in Universal Time
* `dd if=/dev/urandom of=/dev/diskOrPartition bs=1M`
  * Overwrite disk or partition with random bits
* `dd if=/dev/zero of=/dev/diskOrPartition bs=1M`
  * Overwrite disk or partition with zeros
* `df -h`
  * Show file system usage in human-readable format
* `dig -x ip_address`
  * Do a reverse domain lookup by IP
* `dig @dns-server host`
  * Get DNS record for host from a specific dns server (e.g. 8.8.8.8 for Google's)
* `dig host`
  * Get DNS record for a given host
* `dpkg --get-selections`
  * Show installed packages
* `du -h`
  * Show sizes of directories in human-readable format
* `du -s`
  * Show just a summary of directory's size
* `env`
  * Show currently set environment variables
* `export variable=value`
  * Assigns a value to a variable for the current session. Variable can be used in subsequent commands with $variable. Can be made permanent by adding command to a file like .bash_profile
* `sudo fdisk -l`
  * List hard drive info and all partitions
* `fg job_ID`
  * Bring a process running in the background to the foreground
* `file -i file_name`
  * Show text encoding of a file
* `find -iname "file_name"`
  * Same as -name, but case-insensitive
* `find -name ""file_name"`
  * Find file in current directory and its subdirectories
* `find -name "file_name" -exec command {} \;"`
  * Execute a command on files found, where {} represents the file
* `find -name "file_name" -type type`
  * Filter found files by type (f for file, d for directory)
* `find . -type f | wc -l`
  * Count number of files under current directory
* `free -m`
  * Show amount of used and free memory in megabytes
* `fusermount -u ~/yourmountdirectory`
  * Unmount a remote SSH directory
* `gedit`
  * open file for editing in gedit
* `gpg --allow-secret-key-import --import private_key`
  * Import private key into keyring
* `gpg --armor --detach-sig file`
  * Sign a file, creating a separate signature file
* `gpg --armor --export --output file.gpg user_or_key`
  * Export public key to a file for sharing
* `gpg --armor --export-secret-key --output file.gpg user_or_key`
  * Export private key to a file
* `gpg --gen-key`
  * Generate a gpg public/private key pair
* `gpg --import public_key`
  * Import public key into keyring
* `gpg --list-keys`
  * List public keys in your keyring
* `gpg --output out_file --decrypt in_file`
  * Decrypts file to new file
* `gpg --sign --encrypt --recipient recipient_name file`
  * Sign and encrypt a file with a recipient's public key in one step
* `gpg --sign file`
  * Sign a file
* `gpg --verify file.asc file`
  * Verify a signed file with a separate signature file
* `gpg --verify file.gpg`
  * Verify a signed file
* `gpg -r <keyID> --multifile --encrypt`
  * Encrypt all files in directory
* `gpg -r key_id --encrypt in_file`
  * Encrypt file, specifying key ID with -r
* `grep -e "regex" path`
  * Search using extended regex (same as egrep)
* `grep -r string directory`
  * Search recursively in a directory for a string
* `groupadd group`
  * Create a new user group
* `sudo grub-set-default number`
  * Set default OS in Grub to the specified number, starting with zero
* `htop`
  * Improved UI for viewing and controlling processes (may need installation)
* `id user`
  * See info about a user, including groups it's in
* `ifconfig`
  * List details of each network connection (eth0, etc.)
* `jobs -l`
  * List processes running in the background, along with process IDs
* `jpegtran -optimize image.jpg > image.jpg-opt && mv image.jpg-opt image.jpg`
  * Optimize a jpg and replace original with output
* `kill PID`
  * End a process based on its process ID
* `ln -s target shortcut`
  * Create a symbolic link
* `ls -lsa`
  * Show everything (hidden too) with details
* `lsof -P -i -n`
  * Show programs that are accessing the Internet
* `lspci`
  * List all PCI devices
* `lspci -k`
  * List all PCI devices and the drivers that are handling them
* `lspci | grep VGA`
  * See your video card
* `lsusb`
  * List all USB devices
* `md5sum file`
  * Get the md5 hash of a file
* `sudo mkdir -p /media/cdrom`
  * Example of mounting an ISO (1)
* `mke2fs -c /dev/sdxx`
  * Check a partition for bad blocks
* `mkfs -t type /dev/sdxx`
  * Make a filesystem of the given type on the given partition
* `mount`
  * View all mounts
* `mount -M current_directory new_directory`
  * Move a mount point
* `mount -t iso9660 -o ro /dev/cdrom /mnt`
  * Mount a cd-rom (also works for an ISO)
* `mount /dev/device`
  * Mount a device, checking fstab for its mount directory
* `mount directory`
  * Mount to a directory, checking fstab for its mount device
* `sudo mount -o loop ~/Desktop/ubuntu-10.10-alternate-i386.iso /media/cdrom`
  * Example of mounting an ISO (2)
* `sudo mount -t fs-type -o uid=username,gid=username device directory`
  * Mount a device, specifying filesystem type and user/group
* `mysqldump -u user -p database_name > dumpfilename.sql`
  * Create a database backup from MySQL
* `netstat -a --numeric-ports | grep 8321`
  * tell if a port is in use
* `netstat -plantu`
  * Show all listening and established ports TCP and UDP together with the PID of the associated process
* `nmap host_or_ip`
  * Scan a host or IP for networking info (e.g. open ports)
* `passwd`
  * Change your password
* `passwd user`
  * Change another user's password
* `patch -p1 < patch_file`
  * Apply a patch
* `patch -R < patch_file`
  * Undo an applied patch
* `pgrep name`
  * Search for a running process by name
* `popd`
  * Change back to the last directory saved with pushd
* `ps -A`
  * Show all running processes
* `ps -u user`
  * Show all processes running under user
* `ps aux --sort -rss`
  * Sort processes by memory usage, descending
* `sudo -u postgres psql`
  * Enter PostgreSQL CLI as root user.
* `pstree`
  * View running processes based on which processes started which
* `pushd directory`
  * Change to a directory, saving the previous directory to go back to
* `python -m SimpleHTTPServer`
  * Start dev web server serving from the current directory
* `renice priority PID`
  * Change the priority of a currently running process
* `rm -rf /tmp/foo`
  * Recursively remove directories and files
* `rmdir /tmp/foo`
  * Remove directory
* `rsync -rlztv source target`
  * Sync files recursively, preserving symlinks, using compressing, preserving modification times, and providing verbose output
* `scp`
  * copy file to or from remote computer over SSH
* `scp -r`
  * copy a directory, including any subdirectories
* `scp myfile user@host:DestinationFolder`
  * Copy to specific location on server
* `sed -i "s/original/new/g" file`
  * Replace all instances of original with new in file. i = save to same file; s = substitute
* `sudo sh -c 'cat ~/id_rsa.pub >> authorized_keys'`
  * Run cat as sudo
* `echo -n somePassword | sha256sum`
  * Get the SHA (256 bit) hash of a string via echo, with -n to prevent extra \n
* `sha256sum file`
  * Get the SHA (256 bit) hash of a file
* `shred --remove --iterations=50 file`
  * Overwrite file with random bits using 50 passes, removing file afterwards
* `sudo shutdown -h now`
  * Immediately shuts down computer
* `ssh -D local_port -C user@host -p ssh_port`
  * Create proxy tunnel for web browsing
* `ssh user@host -o TCPKeepAlive=yes`
  * Helps keep connection alive during long operations over ssh (e.g. rsync)
* `ssh-keygen -C comment`
  * Create an SSH key with a custom comment
* `sshfs username@host:/remotepath ~/yourmountdirectory`
  * Mount a remote directory via SSH
* `stat file`
  * Show file stats, including size and ownership
* `tar -cvzf archive.tar.gz fileOrDirectory`
  * Create a gzipped tarball of a file or directory; c = create a tar; z = use gzip; f = read from specified file
* `tar -zxvf archive.tar.gz`
  * Open/uncompress gzipped tarball; z = use gzip; x = extract to disk; v = verbose; f = read from specified file
* `top`
  * Dynamically display running processes
* `umount -f directory`
  * Forcefully unmount a mount
* `umount directory -l`
  * Unmount a mount once it is no longer busy
* `sudo update-grub`
  * Update Grub bootloader after modifying /etc/default/grub
* `useradd -g group -d home_directory -s login_shell user`
  * Create a new user, specifying its group, home directory, and login shell
* `usermod -a -G group user`
  * Add a user to a group
* `usermod -g group -d home_directory -s login_shell user`
  * Modify an existing user, specifying its group, home directory, and login shell
* `vi`
  * edit file in vi editor
* `sudo visudo`
  * Safely opens /etc/sudoers for editing
* `w`
  * Show currently logged in users
* `which command`
  * Show the location of a command's source file
