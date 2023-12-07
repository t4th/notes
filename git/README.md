# git

update submodules
```
git submodule update --init --recursive
```
show local settings
```
git config -l --global --show-origin
```
set certificate
```
git config --list | grep http.sslcainfo
git config --global http.sslcainfo "c:/cert.crt"
git config --global http.sslverify true
```
set credentials
```
git config --global user.name "asd"
git config --global user.email asd.asd@asd.asd
```

generate key (https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh)
```
ssh-keygen -t ed25519 -C "your_email@example.com"
clip < ~/.ssh/id_ed25519.pub
ssh-add /c/Users/<user-name>/.ssh/id_ed25519

```

run ssh agent on bash startup .bashrc
```
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```
