# (Not) Anything about selinux

## Tạo selinux type

1. Cài các yêu cầu

        yum install policycoreutils-devel

2. Tạo file myprivate.te

        policy_module(myprivate, 1.0)

        require {
          type user_home_t;
        };

        type mkd_private_t;
        fs_associate(mkd_private_t)
        allow user_home_t mkd_private_t:{dir file} relabelto;

3. Compile nó

        make -f /usr/share/selinux/devel/Makefile myprivate.pp

        Compiling targeted myprivate module
        /usr/bin/checkmodule:  loading policy configuration from tmp/myprivate.tmp
        /usr/bin/checkmodule:  policy configuration loaded
        /usr/bin/checkmodule:  writing binary representation (version 17) to tmp/myprivate.mod
        Creating targeted myprivate.pp policy package
        rm tmp/myprivate.mod.fc tmp/myprivate.mod

4. Install nó

        semodule -i myprivate.pp

5. Uninstall nó

        semodule -r myprivate

## Add user role

https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_a_user_domain



## List context

1. List directory/file

        ls -Z

        -rw-------. username group unconfined_u:object_r:user_home_t:s0 seul_a_moi

2. Xem user context

        id -Z

        unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

3. Xem status của SElinux

        sestatus

        SELinux status:                 enabled
        SELinuxfs mount:                /sys/fs/selinux
        SELinux root directory:         /etc/selinux
        Loaded policy name:             targeted
        Current mode:                   enforcing
        Mode from config file:          enforcing
        Policy MLS status:              enabled
        Policy deny_unknown status:     allowed
        Max kernel policy version:      30

4. Xem Users của SELinux

        semanage user -l

                        Labeling   MLS/       MLS/
        SELinux User    Prefix     MCS Level  MCS Range                      SELinux Roles

        guest_u         user       s0         s0                             guest_r
        root            user       s0         s0-s0:c0.c1023                 staff_r sysadm_r system_r unconfined_r
        staff_u         user       s0         s0-s0:c0.c1023                 staff_r sysadm_r system_r unconfined_r
        sysadm_u        user       s0         s0-s0:c0.c1023                 sysadm_r
        system_u        user       s0         s0-s0:c0.c1023                 system_r unconfined_r
        unconfined_u    user       s0         s0-s0:c0.c1023                 system_r unconfined_r
        user_u          user       s0         s0                             user_r
        xguest_u        user       s0         s0                             xguest_r

5. Xem User login

        semanage login -l

        Login Name           SELinux User         MLS/MCS Range        Service

        __default__          unconfined_u         s0-s0:c0.c1023       *
        root                 unconfined_u         s0-s0:c0.c1023       *
        system_u             system_u             s0-s0:c0.c1023       *
