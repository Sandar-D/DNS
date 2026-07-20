To give the user sudo privileges, they need to be added to the wheel group first. To do this, run the `usermod -aG wheel sandar` in that syntax. 
	-aG flag will append an user (without removing existing users) while -G is adding it to the supplementary (secondary) groups
	- to make sure that wheel group does have sudo privileges, run the `visudo` command and uncomment the `%wheel  ALL=(ALL)   ALL`. The reason why we run `visudo` instead of modifying the `/etc/sudoers` file on our own is for safety reasons, because if we break that file sudo stops working. 

USERADD

Description:
The useradd command creates a new user account. The login parameter must be a unique string (its length is can be configured by administrators using the chdev command). You cannot use the ALL or default keywords in the user name.

The useradd command does not create password information for a user. It initializes the password field with an asterisk (*). Later, this field is set with the passwd or pwdadm command. New accounts are disabled until the passwd or pwdadm commands are used to add authentication information to the /etc/security/passwd file.

The useradd command always checks the target user registry to make sure the ID for the new account is unique to the target registry. The useradd command can also be configured to check all user registries of the system using the dist_uniqid system attribute. The dist_uniqid system attribute is an attribute of the usw stanza of the /etc/security/login.cfg file, and can be managed using the chsec command.

Flags:
    -c comment - Supplies general information about the user specified by the login parameter.
    -d dir - Identifies the home directory of the user specified by the login parameter. The dir parameter is a full path name.
    -e expire - Identifies the expiration date of the account. The expire parameter is a 10-character string in the MMDDhhmmyy form, where MM is the month, DD is the day, hh is the hour, mm is the minute, and yy is the last 2 digits of the years 1939 through 2038. All characters are numeric. If the expire parameter is 0, the account does not expire. The default is 0. See the date command for more information.
    -g group - Identifies the user's primary group.
    -G group1,2 - Used for setting up secondary groups.
    -k skel_dir - COpies default files from skeleton directory to user's home direcotry. Used only with -m flag.
    -m - Makes user's home directory if it doesn't exist. The default is not to make the home directory.
    -r role1, role2 - Lists the administrative roles for the user. The parameter is a list of role names, separated by commas.
    -s shell Defines the program run for the user at session initiation. The shell parameter is a full path name.
    -u uid - SPecifies the user ID. The uid parameter isa  unqiue integer string. Avoid changing this attribute so that system security is not compromsied.

Files:
THe useradd command has read and write permissions to the following files.
    /etc/passwd
    /etc/group

USERMOD

Description:
The usermod command changes attributes for the user identified by the login parameter. The user name must already exist. To change an attribute, specify the flag and the new value.

Flags:
    -l new_name - Specifies a new name for the user.
    -c comment - Supplies general information about the user specified by the login parameter.
    -d dir - Identifies the home directory of the user specified by the login parameter. The dir parameter is a full path name.
    -e expire - Identifies the expiration date of the account. The expire parameter is a 10-character string in the MMDDhhmmyy form, where MM is the month, DD is the day, hh is the hour, mm is the minute, and yy is the last 2 digits of the years 1939 through 2038. All characters are numeric. If the expire parameter is 0, the account does not expire. The default is 0. See the date command for more information.
    -g group - Identifies the user's primary group.
    -G group1,2 - Used for setting up secondary groups.
    -k skel_dir - COpies default files from skeleton directory to user's home direcotry. Used only with -m flag.
    -m - Makes user's home directory if it doesn't exist. The default is not to make the home directory.
    -r role1, role2 - Lists the administrative roles for the user. The parameter is a list of role names, separated by commas.
    -s shell Defines the program run for the user at session initiation. The shell parameter is a full path name.
    -u uid - SPecifies the user ID. The uid parameter isa  unqiue integer string. Avoid changing this attribute so that system security is not compromsied.

USERDEL

Description:
The userdel command removes the user account identified by the login parameter. The command removes a user's attributes without removing the user's home directory by default. The user name must already exist. If the -r flag is specified, the userdel command also removes the user's home directory.
