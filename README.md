1000k.github.io.src
====

How to deploy
----
### Initializing repository (only at first time)

```sh
cd working-directory/

git clone https://github.com/1000k/1000k.github.io.src
git clone https://github.com/1000k/1000k.github.io.src.git

cd 1000k.github.io.src/

# add any contents...

rm -rf public/
git init
git remote add origin https://github.com/1000k/1000k.github.io.git

hugo
git add -A
git commit -m "Initial commit"
git push origin master
```

### Update

```sh
cd 1000k.github.io.src/

# update contents...

hugo
git submodule add https://github.com/1000k/1000k.github.io.git public
```
