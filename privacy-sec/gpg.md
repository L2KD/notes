Import GPG keys

You have many options to achieve this.

- Import from a key server.
- Import via `keybase`.
- Import manually.

## Manually

Go to https://github.com/voldedore.gpg you'll see the public key of the user `voldedore`.

Run this command to import it to your local machine keyring.

    curl https://github.com/voldedore.gpg | gpg --import

## Config git for signing with GPG

    git config --global user.name Name
    git config --global user.email mail@mail # This mail should match GPG key holder's mail

    gpg2 --list-keys

    ‣ gpg2 --list-keys                  
    /home/admin/.gnupg/pubring.kbx
    ------------------------------
    pub   rsa4096/0x8322BDB26DADE0F0 2019-07-23 [SC]
	  Key fingerprint = 5681 9DB6 25CD A73D 2142  5A6A 8322 BDB2 6DAD E0F0
    uid                   [ unknown] The-Vinh VO <vinh.vo30@gmail.com>
    sub   rsa4096/0x6CFA4751E0188CA2 2019-07-23 [S]
    sub   rsa4096/0x057B3781278A77F2 2019-07-23 [E]
    sub   rsa4096/0x9F387A2DD5516F42 2019-07-23 [A]

Then 

    git config --global user.signkey 6CFA4751E0188CA2

This may be set too

    export GPG_TTY=$(tty)

