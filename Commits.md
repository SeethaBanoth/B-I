  # **Commits**
---

## 1️⃣ What is a Commit?

A **commit** is a **record of changes made to the source code** in a version control system (Android uses **Git**).

It saves:

* What code changed
* Who changed it
* When it was changed
* Why it was changed

You can think of it like a **snapshot of the code at a specific time**.

Example:

```text
Commit ID: a1b2c3d
Author: Developer
Date: 2025-01-10
Message: Added CPU headroom API
```

---

## 2️⃣ Simple Example

Suppose a file exists:

```cpp
// File: calculator.cpp
int add(int a, int b){
    return a + b;
}
```

Now a developer improves it:

```cpp
int add(int a, int b){
    int result = a + b;
    return result;
}
```

This change becomes a **commit**.

Commit record:

```text
Commit message: Improved add() function readability
Files changed: calculator.cpp
```

---

## 3️⃣ Structure of a Commit

Every commit contains several parts.

### 1. Commit ID (Hash)

Example:

```
e7f8a91c5f3a
```

This uniquely identifies the commit.

---

### 2. Author

The developer who made the change.

```
Author: John Doe
```

---

### 3. Date

When the change was made.

```
Date: Tue Feb 4 2025
```

---

### 4. Commit Message

Explains **why the change was made**.

Example:

```
Add CPU headroom API for performance monitoring
```

---

### 5. Code Difference (Diff)

Shows **exactly what changed**.

Example:

```diff
- int result = a + b;
+ int result = a + b + 0;
```

---

## 4️⃣ Example of an AOSP Commit

Example commit from Android source.

```
commit 4d21a93
Author: Android Developer
Date: 2025-03-10

Add CPU headroom API
```

Changed file:

```
frameworks/base/core/java/android/os/PowerManager.java
```

Code change:

```java
public int getCpuHeadroom() {
    return nativeGetCpuHeadroom();
}
```

This commit **adds a new API**.

---

## 5️⃣ Why Commits Are Important

Commits help developers:

### Track changes

You can see **who changed what**.

### Fix bugs

If a bug appears, developers can find **which commit caused it**.

### Restore older versions

You can revert to previous commits.

### Collaboration

Many developers can work on the same project.

---

## 6️⃣ Commit Process (Step by Step)

Typical workflow in Android development:

### Step 1 – Modify Code

Developer edits a file.

Example:

```
frameworks/base/services/display/DisplayManagerService.java
```

---

### Step 2 – Add Changes

Command:

```bash
git add .
```

This prepares files for commit.

---

### Step 3 – Create Commit

Command:

```bash
git commit -m "Add adaptive refresh rate API"
```

Now the change becomes a **commit**.

---

## 7️⃣ Example of Multiple Commits

Project history might look like this:

```
Commit 1: Initial framework setup
Commit 2: Add camera service
Commit 3: Fix memory leak
Commit 4: Add CPU headroom API
```

Each commit is a **step in development**.

---

## 8️⃣ How Commits Help in Android Version Comparison

When comparing **AOSP 15 and AOSP 16**, differences come from **many commits**.

Example:

```
Android 15 → commit history up to X
Android 16 → commit history up to Y
```

Android 16 includes **additional commits**.

Example:

```
Android 15
 ├ commit A
 ├ commit B
 └ commit C

Android 16
 ├ commit A
 ├ commit B
 ├ commit C
 ├ commit D
 └ commit E
```

Commits **D and E** are new features.

---

## 9️⃣ Backporting Using Commits

When integrating features from Android 16 to Android 15:

Steps:

1. Find the commit that introduced the feature.
2. Copy that commit to the Android 15 branch.
3. Fix dependencies.
4. Build and test.

Example command:

```bash
git cherry-pick <commit-id>
```

This copies the commit to another branch.

---

## 🔟 Real Example of Backporting

Suppose Android 16 added:

```
CPU headroom API
```

Commit:

```
commit 92fd8e
Add CPU headroom API
```

Integration engineer runs:

```bash
git cherry-pick 92fd8e
```

Now the **same change appears in Android 15**.

---

## Simple Analogy

Think of commits like **saving versions of a document**.

Example:

```
Document v1 → first commit
Document v2 → second commit
Document v3 → third commit
```

You can always **go back to any version**.

---


# **millions of commits** 
---

## 1️⃣ How Many Commits Android Has

Android source code is divided into **many Git repositories** such as:

```
frameworks/base
system/core
packages/apps
bionic
kernel
hardware/interfaces
```

Each repository has **its own commit history**.

Example (approximate scale):

| Repository      | Approx commits |
| --------------- | -------------- |
| frameworks/base | 200k+          |
| system/core     | 40k+           |
| bionic          | 30k+           |
| packages/apps   | 100k+          |
| kernel          | 500k+          |

So overall Android has **millions of commits**.

---

## 2️⃣ What Types of Commits Exist

Commits are usually categorized by **what they change**.

### 1. Feature Commits

Add new functionality.

Example:

```
Add camera API
Add CPU headroom API
Add new display feature
```

These introduce **new capabilities**.

---

### 2. Bug Fix Commits

Fix errors in existing code.

Example:

```
Fix memory leak in ActivityManager
Fix crash in camera service
```

These are **very common commits**.

---

### 3. Security Commits

Fix security vulnerabilities.

Example:

```
Fix privilege escalation vulnerability
Fix buffer overflow in media framework
```

These are **very important commits**.

---

### 4. Performance Commits

Improve speed or efficiency.

Example:

```
Optimize memory allocation
Improve boot time
Reduce CPU usage
```

---

### 5. Refactoring Commits

Change internal code structure **without changing behavior**.

Example:

```
Refactor power manager code
Clean up unused functions
```

---

## 3️⃣ How Commits Work Internally

Git stores commits as a **chain of snapshots**.

Example history:

```
Commit A → Commit B → Commit C → Commit D
```

Each commit contains:

```
commit id
author
date
commit message
code changes
```

When developers build Android, the **latest commit state** is used.

---

## 4️⃣ How Commits Become New Android Versions

Example:

```
Android 15
 ├ commit A
 ├ commit B
 └ commit C

Android 16
 ├ commit A
 ├ commit B
 ├ commit C
 ├ commit D
 └ commit E
```

Commits **D and E** are new changes added after Android 15.

---

## 5️⃣ Safe vs Unsafe Commits (Important in Build & Integration)

When integrating commits, engineers classify them.

---

### Safe Commits

These are usually safe to merge.

Examples:

1. Bug fixes
2. Security patches
3. Documentation updates
4. Minor improvements

Example commit message:

```
Fix null pointer crash in camera service
```

These normally **do not affect other components**.

---

### Risky / Unsafe Commits

These can break the system.

Examples:

1. Kernel changes
2. Major framework modifications
3. Hardware interface changes
4. API changes

Example:

```
Change display HAL interface
```

If you integrate this incorrectly:

```
Framework ↔ HAL mismatch
```

The system may fail to boot.

---

## 6️⃣ How Integration Engineers Choose Commits

Typical process:

```
1. Identify feature commit
2. Check dependencies
3. Check affected modules
4. Integrate commit
5. Build
6. Test
```

Command example:

```
git cherry-pick <commit-id>
```

This copies the commit into another branch.

---

## 7️⃣ Example in Android Build Work

Suppose Android 16 added:

```
CPU headroom API
```

Integration engineer:

```
Find commit
Check dependencies
Cherry-pick commit
Fix compile errors
Build system
Test feature
```

---

## 8️⃣ Simple Way to Remember

You can think of commits like **versions of a book**.

```
Version 1 → commit
Version 2 → commit
Version 3 → commit
```

Android development history is basically **a long chain of commits**.

---


# **types of commits** 
---

## 1. Feature Commit (`feat`)

Adds a **new feature** to the project.

Example:

```
feat: add dark mode support
```

✔ Usually safe if isolated
⚠ Might affect other modules if APIs change

---

## 2. Fix Commit (`fix`)

Fixes a **bug or defect** in the code.

Example:

```
fix: resolve crash in camera service
```

✔ Usually safe
✔ Often cherry-picked to older versions

---

## 3. Refactor Commit (`refactor`)

Changes the **internal code structure without changing behavior**.

Example:

```
refactor: simplify media service initialization
```

✔ Mostly safe
⚠ Risk if refactor touches many modules

---

## 4. Documentation Commit (`docs`)

Changes **documentation only**.

Example:

```
docs: update API usage guide
```

✔ Completely safe
✔ No code behavior changes

---

## 5. Style Commit (`style`)

Code formatting changes only.

Example:

```
style: fix indentation in Bluetooth service
```

✔ Safe
✔ No functional changes

---

## 6. Performance Commit (`perf`)

Improves **performance** without changing functionality.

Example:

```
perf: optimize binder thread handling
```

⚠ Usually safe
⚠ But can affect timing-sensitive modules

---

## 7. Test Commit (`test`)

Adds or modifies **tests**.

Example:

```
test: add unit tests for audio manager
```

✔ Safe
✔ Only affects testing

---

## 8. Chore Commit (`chore`)

Maintenance tasks not affecting source logic.

Example:

```
chore: update build scripts
```

✔ Usually safe
⚠ Might affect build system

---

## 9. Build Commit (`build`)

Changes to **build system or dependencies**.

Example:

```
build: update gradle version
```

⚠ Risky for integration across versions

---

## 10. Revert Commit (`revert`)

Undo a previous commit.

Example:

```
revert: remove unstable power HAL change
```

⚠ Must verify dependency

---

## Safe vs Risky for Integration (AOSP → Downstream)

### Safest commits

These rarely break other modules:

* `docs`
* `style`
* `test`
* small `fix`

### Medium risk

Need review:

* `refactor`
* `perf`
* `chore`

### High risk

Often break builds:

* `feat`
* `build`
* API changes
* framework architectural changes

---

## Example Safe Cherry-Pick Candidates

When integrating changes from Android 16 into Android 15, typically look for:

* crash fixes
* security patches
* small service bug fixes

Avoid:

* framework API changes
* HAL interface updates
* major system service rewrites

---

✅ **Simple rule used by AOSP maintainers:**

> Small `fix` commits touching a single file or module are safest to cherry-pick.

---

