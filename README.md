This is an experimental work.

the idea is:
- keep updated the media_tree from https://git.linuxtv.org/media_tree.git every hour...
- apply our patches to some different branches (vuplus-media-tree patch)
- rename few function to avoid conflicts with other modules
- prepare a scheduled (or updated on push) action to prepare a release of our branch to avoid on build time to download 2gb git of linux sources


Development:
============

git clone git://git.linuxtv.org/media_build.git 
cd media_build 
git checkout ef54f05ca46befdef5b9159c7e56066f12dc015a -B master 

git clone https://github.com/BlackHole-Devel/media_tree.git media 

do whatever you want on our fork... 

syncronize modified kernel source with media_build..

make -C linux dir DIR=../media/ 

build media-tree (TBD)

since having a full history of the kernel is huge, we need to push just media tree somewhere... (github releasE?!

follow a snippet from blackhole(legacy) git push hook
echo "Hello from $0 hook..." 
echo "Creating temp directory" 
temp_dir=$$ 
mkdir -p /home/git/temp/$temp_dir/ 
cd /home/git/temp/$temp_dir/ 
echo "Cloning media_build" 
git clone -b ${branch} /home/git/repos/bh/media_build.git 
cd /home/git/temp/$temp_dir/media_build/ 
echo "Cloning bh patched linuxtv kernel (branch ${branch})" 
git clone -b ${branch} /home/git/repos/bh/vuplus_kernel.git /home/git/temp/$temp_dir/media_build/media 
#make -C linux tar DIR=../media/ 
echo "Preparing tar for media-tree_git.bb" 
make -C linux tar DIR=/home/git/temp/$temp_dir/media_build/media > /home/git/temp/$temp_dir/make.log 2>&1. 
[[ $? -gt 0 ]] && tail -n 25 /home/git/temp/$temp_dir/make.log 
mkdir -p /home/git/www/archive 
echo "Copy tar to htdocs" 
rm -f /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2 
rm -f /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2.md5 
mv linux-media.tar.bz2 /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2 
md5sum /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2 > /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2.md5 
chmod 666 /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2 /home/git/www/archive/vuplus_kernel-${branch}-latest.tar.bz2.md5 
echo "Remove temp directory" 
rm -rf /home/git/temp/$temp_dir/ 
echo "vuplus_kernel-${branch}-latest.tar.bz2 ready :-)" 


