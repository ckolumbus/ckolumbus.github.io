CKolumbus' Blog Source
======================

This is the source for [my blog on GitHub
pages](https://ckolumbus.github.io).


## How to build / update

This is a summary based on the [hugo post](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
on how to publish to hugo sites to `gh-pages`.

```
git clone -b BlogSource --recursive https://ckolumbus@github.com/ckolumbus/ckolumbus.github.io.git
cd ckolumbus.github.io
git worktree add -B master public origin/master
```

now add/edit the posts and commit the changes to `BlogSource` branch.
After this the new site can be published via:

```
cd public 
git add --all 
git commit -m "Publishing to gh-pages" 
git push origin master
cd ..
```


