all: vm

Core-current.iso:
	curl -OL http://tinycorelinux.net/6.x/x86/release/Core-current.iso

vmlinuz core.gz: Core-current.iso
	hdiutil mount Core-current.iso
	cp /Volumes/Core/boot/$@ . && sync
	hdiutil unmount /Volumes/Core

initrd.gz: core.gz
	mkdir -p initrd
	( cd initrd ; sudo tar -xzf ../core.gz | sudo cpio -idm )
	sudo gsed -i '/# ttyS0/c\ttyS0' initrd/etc/securetty
	sudo gsed -i '/^tty1:/s#tty1#ttyS0#g' initrd/etc/inittab
	( cd initrd ; find . | sudo cpio -o -H newc ) | gzip -c > initrd.gz
	sudo rm -rf initrd

vm: vmlinuz initrd.gz
	mkdir VM
	cp config.yml VM/
	mv vmlinuz VM/
	mv initrd.gz VM/

clean:
	$(RM) vmlinuz core.gz
	$(RM) Core-current.iso
	$(RM) -r initrd
	$(RM) initrd.gz
	$(RM) -r VM/
