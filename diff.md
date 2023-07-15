# Diff

```bash
# Create a patch file
diff -u file1.html file2.html > patchfile.patch

# Apply patch
patch file1.html patchfile.patch

# Reverse a patch
patch -p0 -R -i patchfile.patch
```

## Diff two directories

```bash
diff --color=always -burN dir1/ dir2/
```
