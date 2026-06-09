Enter the following to enable storing PW/PAT securely:
```sh
git config --global credential.helper libsecret
```

If you want to use the same PW/PAT for the url:
```sh
git config --global credential.useHttpPath false
```