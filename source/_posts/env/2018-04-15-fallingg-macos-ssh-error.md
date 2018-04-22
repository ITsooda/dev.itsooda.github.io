---
title:  "맥os에서 ssh 접속시 등장하는 에러 해결"
date:   2018-04-15
authorId: fallingg
comments: true
tags: [fallingg, ssh, macos, env]
layout: post
categories: "env"
---





최근 ssh로 서버에 붙어 작업할 일이 많아지면서 자연스레 맥 터미널에서 ssh를 사용하는 빈도가 함께 증가하였는데, centos에 접속할 때는 별다른 에러가 발생하지 않지만, 우분투에 접속할 때는 항상 뜨는 에러가 있었다(이게 OS를 가리는 문제인지는 모르겠지만, 경험과 지식이 짧은 내 입장에선 그렇게 받아들여졌다).



```bash
Unable to negotiate with ***.***.***.*** port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1
```



아무런 지식이 없는 상태에서 해석을 해보자면, 서버에서 요구하는 어떤 key exchange method들을 내 맥OS에서 지원하지 않는다는 이야기로 받아들였다. 역시나 구글링이 필요한 시점. 처음 찾았던 해결책은 ssh command에서 옵션으로 key exchange algorithm을 지정해서 접속하는 방법이었다.



- 기존 접속 command : `ssh id@***.***.***.***`
- 해결 접속 command : `ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -caes128-cbc id@***.***.***.***`



매번 하다보니 너무 귀찮기도 했고, 어느 정도(?) ssh나 리눅스를 건드려보면서 직접 접속할 때 config 파일을 수정하여 저 과정을 줄일 수 있지 않을까 생각했고, 맥os상의 /etc/ssh/ssh_config 파일을 수정하여 답을 찾았다. 주석 처리가 되어 있는 Ciphers 및 MACs의 주석을 제거하고, 맨 아래 HostkeyAlgorithms과 KexAlgorithms 관련 내용을 추가해주었다.



```shell
# Host *
#   ForwardAgent no
#   ForwardX11 no
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   IdentityFile ~/.ssh/id_ecdsa
#   IdentityFile ~/.ssh/id_ed25519
#   Port 22
#   Protocol 2
    Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
    MACs hmac-md5,hmac-sha1,umac-64@openssh.com
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
#   RekeyLimit 1G 1h

Host *
        SendEnv LANG LC_*

HostkeyAlgorithms ssh-dss,ssh-rsa
KexAlgorithms +diffie-hellman-group1-sha1
```



뭔지 제대로 모르고 config 파일을 건드는 건 상당히 위험한 듯 하지만, 당장 눈 앞의 문제를 해결하는 것이 우선인 우리네 세상에 그런 거 기다릴 시간이 어디 있는가. 공부할 내용이 하나 늘었다.

끝.

