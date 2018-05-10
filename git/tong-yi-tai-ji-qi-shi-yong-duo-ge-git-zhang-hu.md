# 同一台机器使用多个git账户

用ssh-keygen生成两对key

```text
ssh-keygen -t rsa -C "your-email-address"
```

.ssh/config配置不同的identityFile

```text
Host github.com
HostName ssh.github.com
Port 443
IdentityFile    /root/.ssh/id_rsa

Host git.xx.com
HostName        git.xx.com
User            git
IdentityFile    /root/.ssh/id_rsa_xx
```

