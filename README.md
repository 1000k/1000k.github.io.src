1000k.github.io.src
====

How to deploy
----
### Initializing repository (only at first time)

```sh

cd 1000k.github.io.src
# add any contents...

rm -rf public/
git init
git remote add origin https://github.com/1000k/1000k.github.io.src.git
git add -A
git commit -m "Update"
git push origin master
```

### Update

```sh
cd 1000k.github.io.src

# update contents...

hugo
git submodule add https://github.com/1000k/1000k.github.io.git public
```
