# sftp_configuration
Configuration the sftp connection to the server.

### - create a group for sftp users
	sudo groupadd sftp

### - create the user belonging to this group
	sudo useradd username -g sftp
	sudo passwd user-password

### - open servers sshd config file with your favorit redactor, usually /etc/ssh/sshd_config
	sudo vim /etc/ssh/sshd_config

### - comment this line
	#Subsystem	sftp	/usr/libexec/openssh/sftp-server

### - add to the tail
	Subsystem sftp internal-sftp

	Match Group sftp

		ChrootDirectory path/to/your/sftp/root/directory
	
		ForceCommand internal-sftp
	
		X11Forwarding no
	
		AllowTCPForwarding no
	
		PasswordAuthentication yes
 
### - create directory for your sftp root directory
	sudo mkdir path/to/your/sftp/root/directory

### - make root the owner and group owner of the sftp root directory
	sudo chown root:root path/to/your/sftp/root/directory

### - give permissions to other users only to read and execute in this directory 
	#without this restrictions user will not be able to connect through sftp
	sudo chmod 755 path/to/your/sftp/root/directory

### - restart sshd
	sudo service sshd restart

### Notes:
chroot directory and all its parents should have at max 755 permissions

ChrootDirectory in the sshd config file may be %h, when %h is determined for the user,
thus we have to change user's directory, by using -d, to needed directory which will be used  // sudo usermod -d /dir username
