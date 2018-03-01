List all groups

    getent group | cut -d: -f1

Add a group

    groupadd <group_name>

Add a user to a group

    usermod -a -G <group_name> <username>

Remove a user from a group

    gpasswd -d <username> <group_name>
