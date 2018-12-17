# How to make this repo auto commit

1.  Looking in this location: `.config/` you will find an executable `auto-commit.sh`

2.  If you can't find it. Create one.

        #!/bin/zsh

        cd ~/Projects/notes

        git add .
        git commit -m "Auto update"

    If it's not, chmod it to make it executable

        chmod u+x .config/auto-commit.sh

3.  Edit your `crontab`

        crontab -e

4.  Enter the following to the end of your existing crontab

        */5 * * * * /bin/bash -l -c "~/Projects/notes/.config/auto-commit.sh >> /tmp/git-notes.log 2>&1"

Of course, change `~/Projects/notes` as this repo location (where you've cloned it to). Your mileage may vary.

## Worth mentioning

You might need to add a no-password-required ssh key to your github.com account setting to be able to make the automatically pushing works.
