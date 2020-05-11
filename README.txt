POC


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
Suppose alpha, beta and gamma are three build targets that live in a repo.

Note also the slightly wacky dependency structure: gamma depends upon alpha, beta and epsilon. There's a literal copy of the epsilon dependnecy, but the dependencies on alpha and beta are expressed using symbolic links containing relative paths to the corresponding directories inside the repo root.

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

