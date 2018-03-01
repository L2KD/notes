List all groups

    getent group | cut -d: -f1

Add a group

    groupadd <group_name>

Add user to a group 

    usermod -a -G <group_name> <username>
