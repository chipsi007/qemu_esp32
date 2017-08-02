Work in progress, allow gdb to connect over wifi on a live system
wifistub.c is the important file, the other files comes from here,
https://github.com/blacksphere/blackmagic

The idea is to add extended-remote and
(gdb) load filename.bin capabilities.

Some useful monitor commands would also be nice to be able to fix
the annoying qemu flash emulation bug.


xtensa-esp32-elf-gdb build/wifigdb.elf   -ex 'target remote 192.168.4.3:2345'


Allows reading from memory on a live system,


xtensa-esp32-elf-gdb build/wifigdb.elf   -b 115200 -ex 'target remote /dev/ttyUSB0'



The purpose of this project is to understand the gdb-stub and maybe improve it with some OTA flashing capabilities.

https://www-zeuthen.desy.de/dv/documentation/unixguide/infohtml/gdb/Thread-List-Format.html

As all other code here, its for learning.


In qemu
======

xtensa-softmmu/qemu-system-xtensa -d unimp,guest_errors -cpu esp32 -M esp32 -m 4M   -net nic,model=vlan0 -net user,id=simnet,net=192.168.4.0/24,host=192.168.4.40,hostfwd=tcp::2345-192.168.4.3:2345  -net dump,file=/tmp/vm0.pcap  -kernel /home/olas/esp/qemu_esp32/examples/26_wifigdb/build/wifigdb.elf -s     >  io.txt


(gdb) set debug remote 1
(gdb) set remotetimeout 30 
(gdb) set debug xtensa 1
(gdb) target remote:2345


Also try 
(gdb) target extended-remote:2345

