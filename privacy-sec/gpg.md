Import GPG keys

You have many options to achieve this.

- Import from a key server.
- Import via `keybase`.
- Import manually.

## Manually

Go to https://github.com/voldedore.gpg you'll see the public key of the user `voldedore`.

Run this command to import it to your local machine keyring.

    curl https://github.com/voldedore.gpg | gpg --import

