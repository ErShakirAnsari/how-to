# Some useful LINUX commands

#### updateall

```
echo "alias updateall='echo -e \"\n----- updating\" && sudo apt update && echo -e \"\n----- upgrading\" && sudo apt upgrade && echo -e \"\n----- Autoremoving\" && sudo apt autoremove'" >> ~/.bashrc
```

and then reload

```
source ~/.bashrc
```
