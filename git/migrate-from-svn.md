0. get authors.txt
by this
    svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt

1. git svn clone
by this
    git svn clone http://svn.repo --no-metadata -A /path/to/authors-transform.txt /destination/git/repo/path/

2. Create bare repo on server

3. remote add origin from repo server

4. git push origin master

4b. git svn fetch
git merge --ff-only git-svn
