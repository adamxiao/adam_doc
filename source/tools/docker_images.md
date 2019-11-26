# docker images

vim env
```bash
docker run -it chxuan/ubuntu-vimplus
# docker image size 38.9MB
alias dvi='docker run -ti --rm -v $(pwd):/home/developer/workspace jare/alpine-vim'
# docker image size 376MB
alias dvi='docker run -ti --rm -v $(pwd):/home/developer/workspace jare/vim-bundle'
docker run -it dekelund/vim-env
```

other tools
```bash
# glusterfs server
docker pull greatsun2009/glusterfs
```

