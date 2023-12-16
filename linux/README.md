# linux
## commands

nohup npm start & - nohup "no hang up" after exit ssh session; & start npm in bg
ps aux | grep npm - to find process id and kill it

cd ~     home dir.
cd -     (minus) previous dir

/usr/bin    dir where linux is looking for apps called from the terminal

ln      linux version of junction: sudo ln -s /source/binary /usr/bin
unlink  remove link

rm      remove. -r recursive. -rf (recursive, force)

touch    create new file, ie. touch file.txt

apt-get install    fetch, update

cat     display file content

less    display file content. Navigate with arrows and quit with Q

wc      word counter. -l count the lines

man    manual. man wc

--help

usage:

wc -l *.txt     cound number of liens in txt files

ls -l    -l long 

clear or ctrl+l

find <where> -iname '*text.txt'    find *

pipe |

find -name 'name' | wc -l    
find -name 'name' > ../list.txt    cmd output redirection

mv <co> .    dot is current dir

chmod +x    change mode to execute

gzip    zip
guzip    unzip
tar -czpf ../output.tar.gz ./    . Tape archive
tar -ztf output.tar.gz | less
tar -zxf output.tar.gz

grep e file.txt     search for 'e' in a file
grep ^t    lines starting with 'T'
grep -E '^T.+n$' file.txt    E as extended, which is regex

cat file*.txt | grep '^k' | sort    find in files and sort

alt + .    repeat the last argument. Ex. mkdir -p asd/ad/22/d, cd alt+

ctrl+u    copy the current command to bash stash
ctrl+y    recover command from stash

ctrl + t     fix errors in command

echo some funny text
^funny^ugly     call previous command with replaced text

ctrl + x,e    command edit
shuf -e {1..10}    generate random numbers

ctrl+shift+c /v

ctrl + r    revers search through bash history

!!    attach sudo to cmd
