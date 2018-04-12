# Homestead 7

1. Cài ioncube, new project, add source, chọn target dir. Nhấn nút Play để tiến hành encode.

2. Download loader về. Giải nén bỏ vào www.

3. Truy cập http://192.168.1.51/ionCube/loader-wizard.php.

4. THằng wizard này sẽ hướng dẫn cách install loader:

  1. Copy cái extension thích hợp (\*.so, có hiện trong thằng wizard) vào `/usr/lib/php/20131226` (có hiện trong wizard luôn).

  2. Download cái file `00-ioncube.ini` về (từ wizard), quăng vào `/etc/php/5.6/fpm/conf.d/` (wizard sẽ chỉ).

  3. Restart dịch vụ php.

  4. Refresh cái wizard, nếu nó nói Loader Installed là ok.

5. Remove cái folder `ionCube` đi (trong www)
