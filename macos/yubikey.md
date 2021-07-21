Everything of 2FA feature should work out-of-the-box.

## Configuration for enabling GPG agent to SSH

_This was tested OK on macOS 11.4 Big Sur_

Install GPGSuite from https://gpgtools.org/gpgsuite.html

Install other prequisites

    $ brew install gnupg yubikey-personalization hopenpgp-tools ykman pinentry-mac

Copy enhanced conf from

    $ cd ~/.gnupg

    $ wget https://raw.githubusercontent.com/drduh/config/master/gpg-agent.conf

    $ grep -ve "^#" gpg-agent.conf
    enable-ssh-support
    default-cache-ttl 60
    max-cache-ttl 120
    pinentry-program /usr/bin/pinentry-curses

Change pinentry program to `pinentry-program /usr/local/MacGPG2/libexec/pinentry-mac.app/Contents/MacOS/pinentry-mac`

Add the following command to your shell `rc` (e.g `.zshrc`)

    export GPG_TTY="$(tty)"
    export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
    gpgconf --launch gpg-agent
    
Everything should work as expected now.

---

DrDUH has more config to use GUI app, didn't test yet, since I was able to do SSH with the above config.

Further read: https://github.com/drduh/YubiKey-Guide#macos-1
