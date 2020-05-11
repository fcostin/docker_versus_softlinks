docker-versus-softlinks example
===============================


Suppose we have a git repo holding a filesystem structured like this:


```
<repo-root>/
	alpha/
		main.txt
	beta/
		main.txt
	gamma/
		main.txt
		vendor/
			alpha -> softlink(../../alpha)
			beta -> softlink(../../beta)
			epsilon/
				main.txt
```
Suppose `alpha`, `beta` and `gamma` are three sibling build targets that live in a repo.

Note also the slightly wacky dependency structure: `gamma` depends upon `alpha`, `beta` and `epsilon`. There's a literal copy of the `epsilon` dependency, but the dependencies on `alpha` and `beta` are expressed using symbolic links containing relative paths to the corresponding directories inside the repo root.

From the perspective of "building" the "gamma" target, can we do that from inside a build container?

In particular, what happens if we try to copy in the softlinks?


### run the demo


```
cd gamma
docker build -f Dockerfile ..
```

note the `..`: we need to tell docker to use the entire repo as the build context, not just `.`, the `gamma` directory. This is because docker refuses to copy files from outside the build context during build, but we need to be able to copy in `alpha` and `beta` (via the relative symbolic links from inside gamma's vendor folder)

### does it work

yes it does work. fantastic

### motivation

alpha, beta, gamma are go modules . gamma wants to import code from alpha and beta, but for historical reasons all the go modules vendor all their dependencies. We can hack around this by adding a directive like

```
replace example.com/project/alpha => ../alpha
````

to `gamma`'s `go.mod` file and then creating a symlink from `gamma`'s `vendor/example.com/project/alpha` containing the relative path to alpha in the root of the dir, which would be something like `../../../../alpha` (there are another two layers of `..` since the link `alpha` sits inside `gamma/vendor/example.com/project` relative to the repo root.
