#! /bin/bash
unlink screen.mpg
unlink camera.mpg
echo "Starting Desktop Grab"
ffmpeg -f x11grab -s `xdpyinfo | grep 'dimensions:'|awk '{print $2}'` -r 25 -i :0.0 -sameq screen.mpg > /dev/null 2>&1 &
echo "Starting Camera Grab"
ffmpeg -f video4linux2 -s 640x480 -i /dev/video0 -sameq camera.mpg  > /dev/null 2>&1 &
read -p "Press any key to stop recording."
ps aux | grep ffmpeg | awk '{print $2}' | xargs kill
