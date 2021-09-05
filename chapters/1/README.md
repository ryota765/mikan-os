# Procedures

Execute in Mac local environment.  
Connecting GUI from docker env was difficult.  

```
(Only first time)
$ brew install dosfstools qemu llvm nasm

(Add path to ~/.zshrc)
$ export PATH=/usr/local/Cellar/llvm/12.0.1/bin:$PATH
$ export PATH=/usr/local/Cellar/dosfstools/4.2/sbin:$PATH

$ cd ${root}/chapters/1
$ qemu-img create -f raw disk.img 200M
$ mkfs.fat -n 'MIKAN OS' -s 2 -f 2 -R 32 -F 32 disk.img

$ hdiutil attach -mountpoint mnt disk.img
$ mkdir -p mnt/EFI/BOOT
$ cp hello.efi mnt/EFI/BOOT/BOOTX64.EFI
$ hdiutil detach mnt

$ curl -O https://raw.githubusercontent.com/uchan-nos/mikanos-build/master/devenv/OVMF_CODE.fd
$ curl -O https://raw.githubusercontent.com/uchan-nos/mikanos-build/master/devenv/OVMF_VARS.fd

$ qemu-system-x86_64 -drive if=pflash,file=OVMF_CODE.fd -drive if=pflash,file=OVMF_VARS.fd -hda disk.img
```

# Memo

- emulatorを使った方針  
P.35のスクリプトでこける -> USB+WindowsPCをテスト機として使う方針に変更  

```
# sudo qemu-system-x86_64 \
    -drive if=pflash,file=$HOME/osbook/devenv/OVMF_CODE.fd \
    -drive if=pflash,file=$HOME/osbook/devenv/OVMF_VARS.fd \
    -hda disk.img

> 
WARNING: Image format was not specified for '/root/osbook/devenv/OVMF_CODE.fd' and probing guessed raw.
         Automatically detecting the format is dangerous for raw images, write operations on block 0 will be restricted.
         Specify the 'raw' format explicitly to remove the restrictions.
WARNING: Image format was not specified for '/root/osbook/devenv/OVMF_VARS.fd' and probing guessed raw.
         Automatically detecting the format is dangerous for raw images, write operations on block 0 will be restricted.
         Specify the 'raw' format explicitly to remove the restrictions.
WARNING: Image format was not specified for 'disk.img' and probing guessed raw.
         Automatically detecting the format is dangerous for raw images, write operations on block 0 will be restricted.
         Specify the 'raw' format explicitly to remove the restrictions.
qemu-system-x86_64: warning: TCG doesn't support requested feature: CPUID.01H:ECX.vmx [bit 5]
Could not initialize SDL(No available video device) - exiting
```

- USBを使う方針

dfコマンドでUSBの名前が確認できる
```
$ df

>
Filesystem    512-blocks      Used Available Capacity iused      ifree %iused  Mounted on
/dev/disk1s5   976490576  21994104 300186936     7%  488399 4881964481    0%   /
devfs                385       385         0   100%     667          0  100%   /dev
/dev/disk1s1   976490576 646508192 300186936    69% 3857108 4878595772    0%   /System/Volumes/Data
/dev/disk1s4   976490576   6291520 300186936     3%       3 4882452877    0%   /private/var/vm
map auto_home          0         0         0   100%       0          0  100%   /System/Volumes/Data/home
/dev/disk1s3   976490576   1033256 300186936     1%      54 4882452826    0%   /Volumes/Recovery
/dev/disk2s1     7895968      1816   7894152     1%       0          0  100%   /Volumes/USB
```

- Docker環境使うとUSBがうまくマウントできなかった
```
volumes: /dev/disk2s1:/dev/disk2s1
privileged: true

-> ディレクトリとしてmountされてしまう
```

# References
[ゼロからのOS自作入門をMacで勉強する方法 (Docker使用)](https://qiita.com/meruneru/items/0b7aa23ea505a80d60f5)
[MacでゼロからのOS自作入門をゼロから勉強中](https://zenn.dev/karaage0703/scraps/b2705131673377)
[Mac で始める「ゼロからのOS自作入門」](https://qiita.com/yamoridon/items/4905765cc6e4f320c9b5)
