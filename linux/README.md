# linux
## commands

```console
nohup npm start & # nohup "no hang up" after exit ssh session; & start npm in bg
```

```console
ps aux | grep npm # to find process id and kill it
```

```console
cd ~   #  home dir.
cd -   #  (minus) previous dir
```

```
/usr/bin    dir where linux is looking for apps called from the terminal
```

```console
ln      # linux version of junction: sudo ln -s /source/binary /usr/bin
unlink  # remove link
```

```console
rm     # remove. -r recursive. -rf (recursive, force)
```

```console
touch   # create new file, ie. touch file.txt
```

```console
apt-get install   # ubuntu fetch, update
```

```console
cat    # display file content
```

```console
less   # display file content. Navigate with arrows and quit with Q
```

```console
man  #  manual. man wc
```

```
--help # standard info flag
```

```console
wc     # word counter. -l count the lines
```

```console
wc -l *.txt    # cound number of lines in txt files
```

```console
ls -l  #  -l long 
```

```console
clear or ctrl+l
```

```console
find <where> -iname '*text.txt'   # find *
```

```
pipe |
```

```console
find -name 'name' | wc -l    
find -name 'name' > ../list.txt   # cmd output redirection
```

```console
mv <co> . # move
```

```console
chmod +x   # change mode to execute
```

```console
gzip    zip
guzip    unzip
tar -czpf ../output.tar.gz ./    . Tape archive
tar -ztf output.tar.gz | less
tar -zxf output.tar.gz
```

```console
grep e file.txt              # search for 'e' in a file
grep ^t                      # lines starting with 'T'
grep -E '^T.+n$' file.txt    # E as extended, which is regex
```

```console
cat file*.txt | grep '^k' | sort   # find in files and sort
```

```console
alt + .  #  repeat the last argument. Ex. mkdir -p asd/ad/22/d, cd alt+
```

```console
ctrl+u   # copy the current command to bash stash
ctrl+y   # recover command from stash
```

```console
ctrl + t    # fix errors in command
```

```console
echo some funny text
^funny^ugly    # call previous command with replaced text
```

```console
ctrl + x,e   # command edit
```

```console
shuf -e {1..10}   # generate random numbers
```

```console
ctrl+shift+c /v
```

```console
ctrl + r   # reverse search through bash history
```

```console
!!   # attach sudo to cmd
```
