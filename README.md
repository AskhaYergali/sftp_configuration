# sftp_configuration
Configuration the sftp connection to the server.

### Create a group for sftp users:
	sudo groupadd sftp

### Create the user belonging to this group:
	sudo useradd username -g sftp
	sudo passwd user-password

### Open servers sshd config file with your favorit redactor, usually /etc/ssh/sshd_config:
	sudo vim /etc/ssh/sshd_config

### Comment this line:
	#Subsystem	sftp	/usr/libexec/openssh/sftp-server

### Add to the tail:
	Subsystem sftp internal-sftp

	Match Group sftp

		ChrootDirectory path/to/your/sftp/root/directory
	
		ForceCommand internal-sftp
	
		X11Forwarding no
	
		AllowTCPForwarding no
	
		PasswordAuthentication yes
 
### Create directory for your sftp root directory:
	sudo mkdir path/to/your/sftp/root/directory

### Make root the owner and group owner of the sftp root directory:
	sudo chown root:root path/to/your/sftp/root/directory

### Give permissions to other users only to read and execute in this directory:
	#without this restrictions user will not be able to connect through sftp
	sudo chmod 755 path/to/your/sftp/root/directory

### Restart sshd:
	sudo service sshd restart

### Notes:
Chroot directory and all its parents should have at max 755 permissions and be owned by the root:root

ChrootDirectory in the sshd config file may be %h, when %h is determined for the user,
thus we have to change user's directory, by using -d, to needed directory which will be used

	sudo usermod -d /needed/sftp/dir/ username

Finally, by configuring permissions to the folders we can limit each user to their own folders.
