0. get authors.txt
by this
    svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt

1. git svn clone
by this
    git svn clone http://svn.repo --no-metadata -A /path/to/authors-transform.txt /destination/git/repo/path/
