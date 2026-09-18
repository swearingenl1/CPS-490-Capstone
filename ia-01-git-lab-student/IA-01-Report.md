
# IA 01 - Git State, History, and Recovery

#### Lucas Swearingen | CPA 490 - Capstone 1 | Dr. Stiffler
 
1. Code: the command or commands you used;
2. Output: the relevant output or other observable evidence; and
3. approximately 2–4 sentences explaining what the evidence demonstrates

## Part 1 — Read Repository State

### Claim 1: HEAD identifies the currently checked-out branch in this scenario.

1. Code: `git symbolic-ref --short HEAD` or `git symbolic-ref HEAD`
2. Output: `main` or `refs/heads/main`
3. `HEAD` is a symbolic reference that identifies which branch reference is currently checked out rather than pointing directly to a commit object. Executing `git symbolic-ref --short HEAD` inspects this pointer and returns `main`. Furthermore, omitting `--short` demonstrates that `HEAD` currently points to the `refs/heads/main` branch reference. This proves `HEAD` tracks the active branch position in the working tree.

### Claim 2: A branch reference identifies a commit.

1. Code: `git rev-parse main` and `git cat-file -t main`
2. Output: `cc72c37b04597b67198ca5c11085492da30f57f7` and `commit`
3. A branch is not a directory, but a named reference that stores a 40-character commit object ID. Running `git rev-parse main` resolves the branch reference name `main` directly to its underlying commit hash. This demonstrates that the branch reference acts as a pointer to a specific commit object within the repository graph.

### Claim 3: A commit identifies a tree and records its parent relationship.

1. Code: `git cat-file -p main`
2. Output: 
```
tree e0adf59fff193c073cafe2114d53803739a43b7c
parent 95cadcebfdc66c5f10fa40d84ee5ab65c92d9648
author Lucas Swearingen <swearingenl1@udayton.edu> 1789691371 -0400
committer Lucas Swearingen <swearingenl1@udayton.edu> 1789691371 -0400

Added Part 1: Claims 1 and 2
```
3. Inspecting the commit object that `main` points to using `git cat-file -p` shows its internal header fields. The `tree` line identifies a tree object representing the snapshot structure of the directory at that point. The `parent` line records the commit object ID of its predecessor, proving that a commit object explicitly stores pointers backward to its parent ancestry.

### Claim 4: The repository contains two branch tips that share earlier history.

1. Code: `git log --graph --oneline --decorate --all`
2. Output: 
3. 
