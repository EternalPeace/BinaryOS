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



