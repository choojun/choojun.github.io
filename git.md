To compute the repository usage in GitHub (pubic repository only)

```
#!/usr/bin/env bash

if [ "$#" -eq 2 ]; then
    echo "$(echo "scale=2; $(curl https://api.github.com/repos/$1/$2 2>/dev/null \
    | grep size | head -1 | tr -dc '[:digit:]') / 1024" | bc)MB"
elif [ "$#" -eq 3 ] && [ "$1" == "-z" ]; then
    # For some reason Content-Length header is returned only on second try
    curl -I https://codeload.github.com/$2/$3/zip/master &>/dev/null  
    echo "$(echo "scale=2; $(curl -I https://codeload.github.com/$2/$3/zip/master \
    2>/dev/null | grep Content-Length | cut -d' ' -f2 | tr -d '\r') / 1024 / 1024" \
    | bc)MB"
else
    printf "Usage: $(basename $0) [-z] OWNER REPO\n\n"
    printf "Get github repository size or, optionally [-z], the size of the zipped\n"
    printf "master branch (`Download ZIP` link on repo page).\n"
    exit 1
fi
```
