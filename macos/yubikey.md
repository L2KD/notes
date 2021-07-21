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

## Config PAM 

_Tested on BigSur_


0. In YubiKey Manager, click Applications > PIV
0. Click Setup for macOS
0. Click Setup for macOS. If you chose Protect with PIN when setting the Management Key, enter your PIN in the prompt. If you set a custom Management Key and did not protect with PIN, enter the Management Key in the prompt.
0. Click OK.
0. Remove your YubiKey and plug it into the USB port
0. In the SmartCard Pairing macOS prompt, click Pair. Note: If this prompt doesn't appear, see the Troubleshooting and Additional Topics section below.
0. In the password prompt, enter the password for the user account listed in the User Name field and click Pair
0. In the SmartCard Pairing prompt, enter the PIN for your YubiKey (refer to the Setting a new PIN section above) and click OK
0. In the "login" keychain prompt, enter your keychain password (typically the password for the logged in user account) and click OK

To test the configuration, lock your Mac (Ctrl+Command+Q), and make sure the password field reads PIN when your YubiKey is inserted. Try unlocking your session with your YubiKey by entering your PIN.

---

DrDUH has more config to use GUI app, didn't test yet, since I was able to do SSH with the above config.

Further read: https://github.com/drduh/YubiKey-Guide#macos-1
