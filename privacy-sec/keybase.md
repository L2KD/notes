# Learning log

Installation

```
sudo pacman -S keybase keybase-gui kbfs
```

Register 1st device

```
keybase signup

Welcome to keybase.io!

   - you are now logged in as voldedore
   - your profile on keybase is https://keybase.io/voldedore
   - type 'keybase help' for more instructions

Found a bug? Please report it with `keybase log send`

Enjoy
```

Add 2nd device (mobile)

```
keybase device add

Starting `device add`...

(Please note that you should run `device add` on a computer that is
already registered with Keybase)
What kind of device are you adding?

(1) Desktop or laptop
(2) Mobile phone

Choose a device type: 2
Scan this QR Code with the keybase app on your mobile phone:
# A QR code
There's also a PNG version in /tmp/keybase_qr.png that might work better.



Or, you can type this verification code into your other device:

	soda neutral skill assist rack bike grief nuclear four



✔ Verification code received.



✔ Success! You added a new device named pomme8 to your account.
```

PGP keys

Import pub key from backup location

```
# cryptsetup open /dev/sdc1 my_usb
Enter your strong passphrase
# mount /dev/mapper/my_usb /somewhere_in_your_pc
# export $GNUPGHOME=/somewhere_in_your_pc/gpg_dir
# gpg --list-keys
# export KEYID=0xSOMEBITHERE
# gpg -a --export $KEYID > /tmp/pubkey.key

Your might want to publish the key to the key server
# gpg --send-key $KEYID
gpg: sending key 0xFF3E7D88647EBCDB to hkps server hkps.pool.sks-keyservers.net

Import the pubkey to our ~/.gpg keyring
On the new terminal (because the old one has had the env variable $GNUPGHOME)
# gpg --import /tmp/pubkey.key
# gpg --list-keys
```

Select the key for keybase

```
$ keybase pgp select --multi
You are selecting a PGP key from your local GnuPG keychain, and
will publish a statement signed with this key to make it part of
your Keybase.io identity.

Note that GnuPG will prompt you to perform this signature.

You can also import the secret key to *local*, *encrypted* Keybase
keyring, enabling decryption and signing with the Keybase client.
To do that, use "--import" flag.

Learn more: keybase pgp help select

#    Algo    Key Id             Created   UserId
=    ====    ======             =======   ======
1    4096R   8322BDB26DADE0F0             The-Vinh VO <vinh.vo30@gmail.com>
Choose a key: 1

If you use your yubikey here, you may have to enter your passphrase (master key)

▶ INFO Generated new PGP key:
▶ INFO   user: The-Vinh VO <vinh.vo30@gmail.com>
▶ INFO   4096-bit RSA key, ID 8322BDB26DADE0F0, created 2019-07-23
```

Sau lệnh này, keybase sẽ publish public key của bạn (gpg key) lên keybase. Nhưng nó sẽ nằm sau cái local keyring của pc đang dùng thực hiện lệnh trên. Mình muốn nó replace luôn pgp key của keybase nhưng chưa biết cách.

More info about the command `keybase pgp select`

```
keybase pgp help select
NAME:
   keybase pgp select - Select a key from GnuPG as your own and register the public half with Keybase

USAGE:
   keybase pgp select [command options] [key query]

DESCRIPTION:
   "keybase pgp select" looks at the local GnuPG keychain for all
   available secret keys. It then makes those keys available for use with keybase.
   The steps involved are: (1a) sign a signature chain link with the selected PGP
   key and the existing device key; (1b) push this signature and the public PGP
   key to the server; and if "--import" flag is passed: (2a) copy the PGP secret half
   into your local Keybase keyring; and (2b) encrypt this secret key with Keybase's
   local key security mechanism.

   By default, Keybase suggests only one PGP public key, but if you want to,
   you can supply the "--multi" flag to override this restriction. If you
   want your secret key imported into the local Keybase keyring, then use
   the "--import" flag. Importing your secret key to Keybase keyring makes
   it possible to use Keybase PGP commands like "pgp decrypt" or "pgp sign".

   If you don't want to publish signature chain link to Keybase servers, use
   "--no-publish" flag. It's only valid when both "--no-publish" and "--import"
   flags are used.

   This operation will never push your secret key, encrypted or otherwise,
   to the Keybase server.

OPTIONS:
   --multi	Allow multiple PGP keys.
   --import	Import private key to the local Keybase keyring.
   --no-publish	Only import to Keybase keyring, do not publish on user profile.
```

Đặt passphrase

```
$ keybase passphrase set

Nhập mật khẩu siêu mạnh của bạn
```

Prove identities

Xác minh chính chủ tài khoản (github, reddit)

Login trên web, chọn prove id, chọn `gpg, curl`.

Thực hiện theo hướng dẫn trên web. (VD github thì cần tạo thêm 1 cái public gist, reddit thì cần comment vào /r của keybase)
