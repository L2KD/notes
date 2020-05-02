Go to a directory then

    for f in *.flac; do ffmpeg -i "$f" -c:v copy -acodec alac "${f%.flac}.m4a"; done
