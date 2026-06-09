---
created: 2026-01-06 12:42:39
tags:
  - note
  - journal
---
1. Make sure git-lfs is installed
	```
	git lfs install
	```
2. Track files with lfs
	```
	git lfs track "*.so"
	```
3. Add and commit .gitattributes
	```
	git add .gitattributes
	git commit -m "Updated .gitattributes"
	```
4. Rewrite history
	```
	git lfs migrate import --include="*.so" --everything
	```
	You can combine multiple includes separated by `,`
5. Clean up and reclaim space
	```
	git reflog expire --expire=now --all
	git gc --prune=now --aggressive
	```
6. Verify lfs is working
	```
	git lfs ls-files
	git lfs status
	```
7. Force push to remote
	```
	git push --force
	```