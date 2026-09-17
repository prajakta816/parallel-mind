# Git Tags — Practical & Industry Notes

## 1. What is a Git Tag?

A **Git tag** is a named reference to a specific Git commit.

Tags are commonly used to mark:

* Release versions
* Production versions
* Stable milestones
* Important commits
* Versions used for deployment

Example:

```text
v1.0.0
   ↓
a4dfc46
   ↓
feat: update TagPractice application
```

Here, `v1.0.0` is the tag and `a4dfc46` is the commit it points to.

### Important concept

A tag does **not contain code separately**.

The code is stored in the commit. The tag simply gives that commit a meaningful name.

---

# 2. Branch vs Tag

## Branch

A branch is a **moving pointer**.

Example:

```text
practice
   ↓
Commit A → Commit B → Commit C
                         ↑
                    latest commit
```

When a new commit is created, the branch moves forward.

## Tag

A tag is generally a **fixed reference** to a particular commit.

```text
v1.0.0
   ↓
Commit B
```

If new commits are created:

```text
v1.0.0
   ↓
Commit B → Commit C → Commit D
```

`v1.0.0` still points to Commit B.

### Interview answer

> A branch is a movable reference used for ongoing development, while a tag is a fixed named reference used to identify a specific commit, commonly for releases or important milestones.

---

# 3. Why Tags Are Used in Industry

Tags provide a reliable way to identify exactly which code was released.

For example:

```text
v1.0.0 → Initial production release
v1.1.0 → New feature release
v1.1.1 → Bug-fix release
v2.0.0 → Breaking release
```

They help with:

* Release management
* Production traceability
* Rollbacks
* Debugging
* CI/CD pipelines
* Identifying deployed versions
* Team communication

Example:

If production is running `v1.1.0`, developers can identify the exact commit associated with that release.

---

# 4. Semantic Versioning

A common versioning convention is:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
1.4.2
```

Meaning:

```text
1 = MAJOR
4 = MINOR
2 = PATCH
```

## MAJOR

Increment when there is a **breaking/incompatible change**.

```text
1.5.0 → 2.0.0
```

## MINOR

Increment when a **new backward-compatible feature** is added.

```text
1.0.0 → 1.1.0
```

## PATCH

Increment for **backward-compatible bug fixes**.

```text
1.1.0 → 1.1.1
```

Example release lifecycle:

```text
v1.0.0
  ↓
New feature
  ↓
v1.1.0
  ↓
Bug fix
  ↓
v1.1.1
  ↓
Breaking change
  ↓
v2.0.0
```

---

# 5. Types of Git Tags

There are two common types:

1. Lightweight tag
2. Annotated tag

---

# 6. Lightweight Tag

A lightweight tag is a simple reference to a commit.

Command:

```bash
git tag testing-tag
```

Verify:

```bash
git tag
```

Example:

```text
testing-tag
v1.0.0
```

Inspect:

```bash
git show testing-tag
```

Lightweight tags are useful for simple/internal references.

They do not contain separate tag metadata such as:

* Tag message
* Tagger information
* Tag creation date

---

# 7. Annotated Tag

An annotated tag is a proper Git tag object containing metadata.

Command:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Where:

```text
-a
```

means annotated tag.

And:

```text
-m
```

provides the tag message.

Verify:

```bash
git tag
```

Expected:

```text
v1.0.0
```

Inspect:

```bash
git show v1.0.0
```

You can see information such as:

```text
tag v1.0.0
Tagger: Prajakta Patil
Date: ...

Release version 1.0.0

commit a4dfc46...
```

### Industry practice

Annotated tags are generally preferred for **release versions** because they contain useful metadata.

---

# 8. Our Practical Example

We modified:

```text
practice/tagpractice/app.js
practice/tagpractice/package.json
```

Then committed them:

```bash
git commit -m "feat: update TagPractice application"
```

Git created:

```text
a4dfc46
```

Commit relationship:

```text
a4dfc46
    ↓
feat: update TagPractice application
```

Then we created:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Final relationship:

```text
v1.0.0
   ↓
a4dfc46
   ↓
feat: update TagPractice application
```

---

# 9. Important: Creating a Tag Does Not Push Code

This is an important practical concept.

Suppose:

```text
Local branch
     ↓
new commit
```

The commit exists locally.

Creating:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

only creates the tag locally.

It does not automatically push the branch or commit to GitHub.

To push the branch:

```bash
git push origin practice
```

To push the tag:

```bash
git push origin v1.0.0
```

Therefore:

```text
git commit
    ↓
Code stored in commit

git tag
    ↓
Commit gets a release label

git push origin practice
    ↓
Branch/commit pushed to GitHub

git push origin v1.0.0
    ↓
Tag pushed to GitHub
```

---

# 10. Why Our App.js Was Not Initially Updated on GitHub

After creating the commit, Git showed:

```text
Your branch is ahead of 'origin/practice' by 1 commit.
```

This meant:

```text
Local practice
      ↓
   a4dfc46
```

but:

```text
GitHub practice
      ↓
   older commit
```

The solution was:

```bash
git push origin practice
```

After pushing:

```text
Local practice
      ↓
   a4dfc46
      ↑
GitHub practice
```

The `app.js` code was then available on GitHub.

### Lesson

A tag and a branch are separate references.

Pushing a tag does not replace the need to push the branch containing the commit.

---

# 11. How to See the Code Stored at a Tag

You can directly retrieve a file from a particular tag.

Command:

```bash
git show v1.0.0:practice/tagpractice/app.js
```

This means:

> Show `app.js` exactly as it existed at version `v1.0.0`.

This is useful when comparing different releases.

---

# 12. Inspect All Tags

Command:

```bash
git tag
```

Example:

```text
v1.0.0
v1.1.0
v1.1.1
```

---

# 13. Inspect a Particular Tag

Command:

```bash
git show v1.0.0
```

This shows:

* Tag information
* Tag message
* Commit information
* Changes associated with the commit

---

# 14. Delete a Local Tag

For a temporary tag:

```bash
git tag -d testing-tag
```

Expected:

```text
Deleted tag 'testing-tag' (...)
```

Then:

```bash
git tag
```

The temporary tag should no longer appear.

---

# 15. Push a Tag to GitHub

Push one specific tag:

```bash
git push origin v1.0.0
```

Expected:

```text
[new tag] v1.0.0 -> v1.0.0
```

To push all local tags:

```bash
git push origin --tags
```

For controlled release management, pushing a specific release tag is often clearer.

---

# 16. Correct Industry Release Workflow

A realistic workflow is:

```text
feature/health-check
        ↓
      coding
        ↓
      testing
        ↓
     commit
        ↓
   Pull Request
        ↓
     develop
        ↓
 integration/testing
        ↓
     Pull Request
        ↓
       main
        ↓
 release commit
        ↓
   v1.1.0 tag
        ↓
 push release tag
        ↓
     deployment
```

The important principle is:

> A production release tag should normally identify the exact commit that was approved and released.

---

# 17. Release Tag Should Not Be Moved

Once:

```text
v1.0.0
   ↓
Commit A
```

has been released, we should not casually move `v1.0.0` to another commit.

Instead, create another version:

```text
v1.0.0 → Commit A
v1.1.0 → Commit B
v1.1.1 → Commit C
```

This preserves release history.

---

# 18. Useful Tag Commands

### List tags

```bash
git tag
```

### Create lightweight tag

```bash
git tag <tag-name>
```

### Create annotated tag

```bash
git tag -a <tag-name> -m "message"
```

### Show tag

```bash
git show <tag-name>
```

### Delete local tag

```bash
git tag -d <tag-name>
```

### Push one tag

```bash
git push origin <tag-name>
```

### Push all tags

```bash
git push origin --tags
```

### Show file from a specific tag

```bash
git show <tag-name>:<file-path>
```

Example:

```bash
git show v1.0.0:practice/tagpractice/app.js
```

---

# 19. Git Tag vs Git Commit vs Git Branch

```text
Commit
  ↓
Actual snapshot of project

Branch
  ↓
Movable reference to commits

Tag
  ↓
Named reference to a specific commit
```

Example:

```text
                 ┌── feature branch
                 │
A ─── B ─── C ─── D
          ↑
        v1.0.0
```

Here:

* `C` = commit
* `feature branch` = moving development reference
* `v1.0.0` = fixed release marker

---

# 20. Interview Questions

### Q1. What is a Git tag?

> A Git tag is a named reference to a specific commit, commonly used to mark releases or important milestones.

### Q2. Why are tags used?

> Tags provide a stable way to identify exact versions of the code, which helps with releases, production traceability, debugging, rollback, and CI/CD.

### Q3. Difference between branch and tag?

> A branch is a movable reference used for ongoing development, whereas a tag is generally a fixed reference used to identify a specific commit.

### Q4. Difference between lightweight and annotated tags?

> A lightweight tag is a simple reference to a commit, while an annotated tag is a Git object containing metadata such as the tagger, date, and message. Annotated tags are commonly used for releases.

### Q5. Does `git push origin practice` push tags?

> No. Branches and tags are separate references. A tag can be pushed explicitly using `git push origin <tag-name>` or all tags using `git push origin --tags`.

### Q6. Does creating a tag create a new commit?

> No. Creating a tag does not create a new commit. It creates a reference pointing to an existing commit.

### Q7. Can a tag contain code?

> Not independently. The code belongs to the commit. The tag points to that commit and therefore identifies the project snapshot associated with that commit.

### Q8. What is Semantic Versioning?

> Semantic Versioning uses the MAJOR.MINOR.PATCH format to communicate breaking changes, new features, and bug fixes.

Example:

```text
2.4.1
│ │ │
│ │ └── PATCH
│ └──── MINOR
└────── MAJOR
```

---

# 21. Practical Commands We Have Used

```bash
git status
git tag
git tag -a v1.0.0 -m "Release version 1.0.0"
git show v1.0.0
git push origin v1.0.0
git push origin practice
git tag testing-tag
git show testing-tag
git tag -d testing-tag
git show v1.0.0:practice/tagpractice/app.js
```

---

# 22. Current Practical Repository State

Our current workflow is based on:

```text
Repository:
parallel-mind

Branch:
practice

Tag:
v1.0.0

Release commit:
a4dfc46

Commit:
feat: update TagPractice application
```

Conceptually:

```text
practice
   │
   ▼
a4dfc46
   │
   └──────── v1.0.0
```

The separate file:

```text
practice/READMEstash.md
```

is still an unstaged working-tree change and was intentionally kept separate from the TagPractice commit.
