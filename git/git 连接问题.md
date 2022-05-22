### `Connection reset by 140.82.114.4 port 22fatal: Could not read from remote repository.Please make sure you have the correct access rights and the repository exists.`

* `git config --local -e`

    change `url = git@github.com:username/repo.git`

    to `url = https://github.com/username/repo.git`

或者

* `~/.ssh/config`

```bash
Host github.com
  Hostname ssh.github.com
  Port 443
```

### `fatal: unable to access 'https://github.com/Post-90sRaccoon/study.git/':L SSL_read: Connection was reset, errno 10054`

* `git config --global http.sslVerify "false"`

### `fatal: unable to access 'https://github.com/Post-90sRaccoon/study.git/': to connect to github.com port 443: Timed out`

* `git config --global --unset http.proxy`
* `git config --global --unset https.proxy`

* `git config --global http.proxy http://127.0.0.1:7890 ` (使用的是 clash 的话)
* ` git config --global https.proxy http://127.0.0.1:7890`

