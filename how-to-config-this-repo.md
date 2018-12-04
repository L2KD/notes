# How to make this repo auto commit

1. Looking in this location: `.config/` you will find an executable `auto-commit.sh`
2. If you can't find it. Create one.

        #!/bin/zsh

        cd ~/Projects/notes

        git add .
        git commit -m "Auto update"

    If it's not, chmod it to make it executable

        chmod u+x .config/auto-commit.sh

3. Edit your `crontab`

        crontab -e

4. Enter the following to the end of your existing crontab

        */5 * * * * ~/Projects/notes/.config/auto-commit.sh

Of course, this uses ~/Projects/notes as this repo location (where you've cloned it to). Your mileage may vary.

---

On Mac, use this crontab

    */5 * * * * /bin/bash -l -c '/Users/thevinh/Projects/dotfiles/git-notes >> /Users/thevinh/log/git-notes.log 2>&1'
