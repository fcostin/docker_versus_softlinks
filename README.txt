POC

assume alpha beta gamma are three apps that live in a repo

Can we have a git repo holding a filesystem structured like this?

From the perspective of building "gamma" app, can we do that from inside a build container? in particular, what happens if we try to copy in the softlinks and the alpha repo contents


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
			delta/
				main.txt
```
