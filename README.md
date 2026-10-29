# Inside .git: objects and hashing

A SKILL.SCH lab that opens up the `.git` directory to show how Git actually stores your work.

## 1. Initialise and explore

```bash
git init inside-git && cd inside-git
ls -a .git
```

| Path | What it holds |
|---|---|
| `HEAD` | Which branch you are on (`ref: refs/heads/main`) |
| `config` | Repository settings |
| `objects/` | Every file, folder and commit, stored by hash |
| `refs/heads/` | One file per branch, holding a commit hash |

## 2. Hash a file

Git names every object by the SHA-1 of its content. Same content, same hash, on any machine.

```bash
echo "Hello Git" > hello.txt
git hash-object hello.txt
# 9f4d96d5b00d98959ea9960f069585ce42b1349a
```

That is the real hash of `hello.txt` in this repo.

## 3. Store it and look inside

```bash
git add hello.txt
find .git/objects -type f
# .git/objects/9f/4d96d5b00d98959ea9960f069585ce42b1349a

git cat-file -t 9f4d96d   # blob
git cat-file -p 9f4d96d   # Hello Git
```

The first two characters of the hash become the folder name; the rest is the file name.

## 4. Commit, then follow the chain

```bash
git commit -m "Add hello"
git cat-file -p HEAD          # commit -> points to a tree
git cat-file -p HEAD^{tree}   # tree   -> points to the blob
```

commit -> tree -> blob: that chain is all a Git snapshot is.

## What I learned

- Git is a content-addressed store: the hash *is* the address.
- Changing one byte changes the hash, which is how Git detects changes.
- Branches are just files in `refs/heads/` holding a commit hash.
