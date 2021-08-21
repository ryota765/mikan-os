# Create OS from scratch

[ゼロからのOS自作入門](http://zero.osdev.jp/)  
[GitHub repo](https://github.com/uchan-nos/mikanos-build)

## Environment setting

Use docker to set environment.  

```
$ ./scripts/setup.sh 
$ docker-compose exec mikanos bash
# ...
```

## Binary edit

Use vim.  

```
vim -b ${target_file}
:%!xxd # convert from binary

(edit...)

:%!xxd -r # convert back to binary
:wq
```
