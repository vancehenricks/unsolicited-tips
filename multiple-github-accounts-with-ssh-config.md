# Setting up Multiple Github Accounts with SSH Config

Lets say we have two github accounts both using
SSH to authenticate. 

Were trying to clone the two.

secret-1 has this url:
```
git@github.com:secret-1/repo1.git
```

secret-2 has this url:
```
git@github.com:secret-2/repo2.git
```

Alias the second entry with some arbitrary hostname could be anything.

In this case for example I alias github.com to 2nd.github.com.

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/secret-1

Host 2nd.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/secret-2
```

When cloning you need to use the alias to tell SSH to use the right secret.


```
git clone git@2nd.github.com:secret-2/repo2.git
```


## Reference
https://stackoverflow.com/a/17158985
