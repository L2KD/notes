# Anything about selinux 

1. Tạo một cái selinux type

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
