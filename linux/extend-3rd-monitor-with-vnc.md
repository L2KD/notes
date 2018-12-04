1. Cài cái dummy xorg package
2. Cài x11vnc
3. Xài lệnh gtf 640 1136 60 để output được cái mode cho xrandr. Trong đó 640 x 1136 là resolution của ip5s.
4. New mode xrandr --newmode "640x1136_60.00" 60.96 640 680 752 864 1136 1137 1140 1176 -HSync +Vsync (lấy từ cái số 3)
5. Add mode xrandr --addmode VIRTUAL1 "640x1136_60.00"
6. Lệnh này để lấy các vùng screen đang hiển thị. xrandr | grep "^$DEVICE.*$" | grep -o '[0-9]_x[0-9]_+[0-9]_+[0-9]_'
7. Chạy cái vnc server x11vnc -forever -multiptr -clip "640x1136+2446+784"

Cchỗ clip là lấy từ output của số 6
