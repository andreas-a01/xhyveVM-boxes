all: vm

initrd.img vmlinuz64: boot2docker.iso
	hdiutil mount boot2docker.iso
	cp /Volumes/Boot2Docker-v1.8/boot/$@ . && sync
	hdiutil unmount /Volumes/Boot2Docker-v1.8

boot2docker.iso:
	curl -OL https://github.com/timfallmk/boot2docker/releases/download/v1.8.1/boot2docker.iso

boot2docker-data.tar.gz:
	curl -OL https://github.com/ailispaw/boot2docker-xhyve/releases/download/v0.6.0/boot2docker-data.tar.gz

boot2docker-data.img: boot2docker-data.tar.gz
	tar zxvf boot2docker-data.tar.gz

vm: initrd.img vmlinuz64 boot2docker-data.img
	mkdir VM
	cp config.yml VM/
	mv vmlinuz64 VM/
	mv initrd.img VM/
	mv boot2docker-data.img VM/

clean:
	$(RM) initrd.img vmlinuz64
	$(RM) boot2docker.iso
	$(RM) boot2docker-data.img
	$(RM) boot2docker-data.tar.gz
	$(RM) -r VM
