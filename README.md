# BinaryOS
Make the own OS, Step from 0 to 1.

empty.img:
    get from the command: dd if=/dev/zero of=empty.img bs=512 count=2880 
    and modify the address of 510, from 0000 to 55aa
protectmode
    cd protect
    nasm  protect2MBR.asm -o protect2MBR.bin 
    dd if=protect2MBR.bin of=protect.img bs=512 count=1 conv=notrunc
    qemu-system-x86 protect.img or bochs
    	r2p2r.asm ：the file from real mode switch to protect mode and switch to real mode.
    	how to build:
    nasm r2p2r.asm -o r2p2r.bin
   
	floppy.img is copy from empty.img, and has a filesystem: 
    mkfs.msdos floppy.img
    mount -o loop floppy.img /mnt
        copy some file to floppy.img
    cp <filename> /mnt
	insert the floppy.img to VM's driver A
        execute the filename
    a:\r2p2r.com

	r32g2r.asm:from ring3 to callgate to realmode
	callgates.asm :use callgates
	usepage.asm: page use and page switch
	idttest.asm: interrupt and idt test

boot
    cd boot
	boot's files can realize boot and load the loader.bin file,and execute the loader file
    boot.asm
	nasm boot.asm -o boot.bin
	dd if=boot.bin of=floppy.img count=1 conv=notrunc
    loader.asm
	nasm loader.asm -o loader.bin
	mount floppy.img and copy loader.bin to the mount file.

assemble
    hello.asm
	test the linux assemble

binaryos:from realmode to protect
├── boot
│   ├── boot.asm  :load loader.bin文件,从loader.bin启动
│   ├── include
│   │   ├── fat12hdr.inc :FAT12 磁盘的头文件
│   │   ├── load.inc :加载地址，页目录地址，程序入口地址 宏
│   │   └── pm.inc :保护模式下宏，门,描述符，选择子类型
│   ├── loader.asm :set GDT,加载kernel.bin文件，跳转到保护模式代码，显示内存并分页，然后复制kernel.bin程序到虚拟地址，并执行
├── floppy.img :软盘镜像
├── include
│   ├── const.h :GDT,IDT大小，8259A控制器端口
│   ├── global.h : 全局变量
│   ├── protect.h : 存储段描述符/系统段描述符,门描述符,描述符类型值说明,存储段描述符类型值说明,中断向量
│   ├── proto.h :klib.asm中函数原型声明
│   ├── string.h : memcpy函数声明
│   └── type.h :类型声明
├── kernel
│   ├── global.c :全局变量
│   ├── i8259.c :初始化8259中断控制器,和中断函数
│   ├── kernel.asm :重新设置GDT,设置中断宏,中断和异常
│   ├── protect.c :初始化中断门，声明中断处理函数，(常见中断和8259中断)
│   ├── start.c :在保护模式下，重新设置GDT,IDT到内核中,初始化中断
├── lib
│   ├── kliba.asm :显示函数
│   ├── klib.c :整形转换为字符串
│   ├── string.asm :memcpy实现
└── Makefile
	


