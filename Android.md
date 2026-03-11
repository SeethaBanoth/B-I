 **Android Architecture** 

It explains how the Android operating system is structured in layers. Each layer has a specific role in running applications on devices like phones or tablets.

Android architecture mainly has **5 layers**:

```
Applications
Application Framework
Android Runtime + Native Libraries
Hardware Abstraction Layer (HAL)
Linux Kernel
```

---

# 1. Applications Layer

This is the **top layer** where all Android apps run.

### Examples

* Phone
* Messages
* Camera
* WhatsApp
* Chrome
* YouTube

### Purpose

* Provides services directly to the **user**.
* Apps use the **Application Framework APIs** to interact with the system.

### Location in AOSP

```
packages/apps/
```

### Example

When you open **Camera App**, it calls camera APIs from the framework.

---

# 2. Application Framework

This layer provides **APIs and system services** for applications.

Developers use these services to build apps.

### Important Services

| Service              | Purpose                    |
| -------------------- | -------------------------- |
| Activity Manager     | Manages activity lifecycle |
| Window Manager       | Manages screen windows     |
| Package Manager      | Manages app installation   |
| Resource Manager     | Handles app resources      |
| Notification Manager | Shows notifications        |
| Location Manager     | Provides GPS location      |

### Example

If an app wants location:

```
App → LocationManager API → Framework → HAL → GPS hardware
```

### Location in AOSP

```
frameworks/base/
```

Important directories:

```
frameworks/base/services/
frameworks/base/core/
frameworks/base/location/
frameworks/base/media/
```

---

# 3. Android Runtime (ART) + Native Libraries

## Android Runtime (ART)

ART executes Android applications.

### Purpose

* Runs **DEX bytecode**
* Memory management
* Garbage collection
* Thread management

### Files

```
/system/lib/libart.so
```

---

## Native Libraries

These are **C/C++ libraries** used by Android system.

### Examples

| Library         | Purpose              |
| --------------- | -------------------- |
| libc            | Standard C library   |
| SSL             | Secure communication |
| SQLite          | Database             |
| OpenGL          | Graphics rendering   |
| WebKit          | Web browser engine   |
| Media Framework | Audio/video playback |

### Location

```
system/lib/
system/lib64/
```

In AOSP source:

```
bionic/
external/
frameworks/av/
```

---

# 4. Hardware Abstraction Layer (HAL)

HAL acts as a **bridge between hardware and Android framework**.

### Purpose

Framework does not directly access hardware.

Instead:

```
Framework → HAL → Device Driver
```

### Example

Camera usage:

```
Camera App
   ↓
Camera Framework
   ↓
Camera HAL
   ↓
Camera Driver
   ↓
Camera Hardware
```

### HAL Location in AOSP

```
hardware/interfaces/
hardware/qcom/
vendor/
```

Example:

```
hardware/interfaces/camera/
hardware/interfaces/audio/
hardware/interfaces/sensors/
```

---

# 5. Linux Kernel

This is the **lowest layer** of Android architecture.

Android is built on **Linux Kernel**.

### Responsibilities

| Component          | Purpose                       |
| ------------------ | ----------------------------- |
| Process Management | Creates and manages processes |
| Memory Management  | Allocates RAM                 |
| Device Drivers     | Controls hardware             |
| Power Management   | Battery control               |
| Security (SELinux) | Access control                |
| Networking         | Internet communication        |

### Driver Examples

* Camera driver
* Audio driver
* Display driver
* Bluetooth driver
* WiFi driver

### Location

```
kernel/
```

Example kernel path:

```
kernel/msm/
```

(msm = Qualcomm chipset)

---

# Full Android Architecture Flow

Example: **Playing music**

```
Music App
   ↓
Media Framework
   ↓
Native Libraries (Media Framework)
   ↓
Audio HAL
   ↓
Audio Driver
   ↓
Speaker Hardware
```

---

# Simple Diagram

```
+-------------------------+
|       Applications      |
| (Phone, Camera, Apps)   |
+-------------------------+

+-------------------------+
|   Application Framework |
| Activity Manager        |
| Window Manager          |
| Package Manager         |
+-------------------------+

+-------------------------+
| Android Runtime (ART)   |
| Native Libraries        |
| SQLite, OpenGL, Media   |
+-------------------------+

+-------------------------+
| Hardware Abstraction    |
| Layer (HAL)             |
+-------------------------+

+-------------------------+
| Linux Kernel            |
| Drivers, Memory, CPU    |
+-------------------------+
```

---

# In Build & Integration (Your Project)

When comparing **Android 15 vs Android 16**, most changes happen in:

```
frameworks/
system/
packages/
hardware/
vendor/
```

Example:

```
diff -r android15/frameworks android16/frameworks
```

Then integrate the new feature into Android 15 and build.

---





**real-time example Opening the Camera App** and see how it goes through the **Android Architecture layers**. 


---

# Real-Time Example: Opening Camera App

When you tap the **Camera icon**, many layers of Android work together.

---

# Step 1: Applications Layer

You click the **Camera App icon** on your phone.

Example apps:

* Camera
* WhatsApp
* Chrome
* Gallery

What happens:

* The **Camera application starts**.
* It requests Android system to open the camera.

Flow:

```
User → Camera App
```

---

# Step 2: Application Framework

Now the app talks to the **Android Framework APIs**.

The **Camera API** is used.

Important system services involved:

* Activity Manager
* Camera Manager
* Window Manager

What happens:

* **Activity Manager** starts the Camera activity.
* **Camera Manager** prepares the camera device.

Flow:

```
Camera App
   ↓
Camera API (Framework)
   ↓
Camera Manager Service
```

Location in Android source:

```
frameworks/base/
frameworks/av/
```

---

# Step 3: Android Runtime (ART)

Now the **Java/Kotlin code of the app runs**.

Android Runtime (ART):

* Executes the app code
* Manages memory
* Handles threads

What happens:

* Camera app code runs in **ART**.

Flow:

```
Camera App Code
   ↓
ART (Android Runtime)
```

---

# Step 4: Native Libraries

The camera needs **image processing and media support**.

Native libraries help here.

Examples:

* Media Framework
* OpenGL
* Camera native libraries

What happens:

* Camera data processing
* Image encoding
* Frame rendering

Flow:

```
Framework
   ↓
Native Libraries
```

Example path:

```
frameworks/av/
system/lib/
```

---

# Step 5: Hardware Abstraction Layer (HAL)

HAL acts as a **translator between software and hardware**.

The framework **cannot directly access hardware**, so it uses HAL.

What happens:

* Camera framework sends request to **Camera HAL**.

Flow:

```
Camera Framework
   ↓
Camera HAL
```

Example location:

```
hardware/interfaces/camera/
vendor/qcom/
```

---

# Step 6: Linux Kernel

The **Linux Kernel controls the camera driver**.

The kernel communicates with the **actual camera hardware**.

What happens:

* Camera driver captures image data
* Sends it back to Android

Flow:

```
Camera HAL
   ↓
Camera Driver
   ↓
Linux Kernel
```

---

# Step 7: Hardware

Finally the **camera sensor captures the image**.

Flow:

```
Linux Kernel
   ↓
Camera Hardware
   ↓
Image Captured
```

---

# Full Flow (Very Simple)

```
User taps Camera App
        ↓
Applications Layer (Camera App)
        ↓
Application Framework (Camera API)
        ↓
Android Runtime (ART runs app code)
        ↓
Native Libraries (media processing)
        ↓
Hardware Abstraction Layer (Camera HAL)
        ↓
Linux Kernel (Camera Driver)
        ↓
Camera Hardware
        ↓
Image captured
```

---

# One More Example (Opening WhatsApp)

```
User opens WhatsApp
        ↓
Application Layer (WhatsApp App)
        ↓
Framework (Activity Manager)
        ↓
ART runs app code
        ↓
Native libraries (networking)
        ↓
HAL
        ↓
Linux Kernel (Network driver)
        ↓
WiFi/Internet hardware
```

---



                       ** changes between the aosp 15 and aosp 16**


I’ll explain the **important changes between Android 15 and Android 16** in a **simple “Before → After → Why” format**, so it’s easy for you to understand (especially useful for **Android build & integration**).

---

# 1. Memory Management Change (16 KB Page Size)

## Before (Old Android versions)

* Android used **4 KB memory page size**.
* Memory was divided into **4 KB blocks**.
* This worked well when phones had **small RAM (2–4 GB)**.

Example:

```
RAM
 ├─ 4KB page
 ├─ 4KB page
 ├─ 4KB page
 └─ 4KB page
```

Problem:

* Modern phones have **12–16 GB RAM**
* Too many small pages → more **page table entries**
* More **CPU overhead**

---

## After (Android 15 / 16 improvement)

Android now supports **16 KB page size**.

```
RAM
 ├─ 16KB page
 ├─ 16KB page
 └─ 16KB page
```

Result:

* Faster app launch
* Better system performance
* Less power consumption

Testing shows improvements like:

* **3% faster app launch**
* **8% faster boot time**
* **4% less power usage** ([Android Developers][1])

---

## Why this change?

Because modern devices have:

* Large RAM
* Faster CPUs
* Larger applications

So larger memory pages **reduce memory management overhead**. ([Android Developers][2])

---

# 2. Profiling and Performance Monitoring APIs

## Before (Android 15)

Apps could collect performance traces using:

```
ProfilingManager
```

But the **app had to start profiling manually**.

Example:

```
App starts
↓
Developer manually starts profiling
↓
Collect performance data
```

Problem:

* Hard to capture **startup issues**
* Hard to capture **ANR (App Not Responding)**

---

## After (Android 16)

Android added **System-triggered profiling**.

Now the **system automatically collects traces**.

Example:

```
App start
↓
System detects slow start / ANR
↓
System automatically records trace
↓
Trace saved for developer
```

This helps developers **debug performance issues easily**. ([Android Developers][3])

---

# 3. CPU and GPU Resource APIs

## Before (Android 15)

Apps could not easily know:

* Available CPU capacity
* Available GPU capacity

Developers had to guess.

Example problem:

* Games overuse CPU
* Phone heats up
* Thermal throttling happens

---

## After (Android 16)

New APIs:

```
getCpuHeadroom()
getGpuHeadroom()
```

Now apps can check:

```
Available CPU power
Available GPU power
```

Example:

```
Game detects low GPU headroom
↓
Reduces graphics quality
↓
Prevents overheating
```

Result:

* Better battery life
* Better gaming performance ([Android Developers][3])

---

# 4. Display Refresh Rate Improvements

## Before (Android 14 / older)

Refresh rate switching was **static**.

Example:

```
Scrolling → 120 Hz
Video → 60 Hz
```

Switching caused **jitter or lag**.

---

## After (Android 15 + improved in Android 16)

Android introduced **Adaptive Refresh Rate (ARR)**.

Now refresh rate adjusts smoothly.

Example:

```
Static screen → 30 Hz
Scrolling → 90 Hz
Gaming → 120 Hz
```

Android 16 added APIs like:

```
hasArrSupport()
getSuggestedFrameRate()
```

Benefits:

* Less battery usage
* Smooth UI scrolling ([Android Developers][3])

---

# 5. Boot Process Improvement

## Before (Older Android)

Every manufacturer had its **own bootloader implementation**.

Example:

```
Samsung → custom bootloader
Xiaomi → custom bootloader
Pixel → different bootloader
```

Problem:

* Hard to maintain
* Hard to update

---

## After (Android 16)

Android introduced **Generic Bootloader (GBL)**.

Concept:

```
Standard bootloader design
```

Benefits:

* Easier updates
* Easier maintenance
* Faster boot process ([Android Open Source Project][4])

---

# 6. Accessibility Improvements

## Before

Accessibility APIs were limited.

Example:

* Hard for **screen readers** to interpret complex UI.

---

## After (Android 16)

Android added new APIs for:

* Text outline contrast
* Better screen reader support
* Multiple labels for UI elements

Example:

```
AccessibilityNodeInfo (multiple labels support)
```

This helps:

* Visually impaired users
* Screen reader tools like TalkBack ([Android Developers Blog][5])

---

# 7. Smart Notifications (User Feature)

Android 16 introduced **AI notification summaries**.

Example:

Before:

```
WhatsApp group:
100 messages
```

After:

```
Summary:
"10 people discussed meeting at 5 PM"
```

This helps users quickly read notifications. ([The Verge][6])

---

# Summary (Very Important for Interviews)

| Area          | Before               | After                      |
| ------------- | -------------------- | -------------------------- |
| Memory        | 4 KB page size       | 16 KB page support         |
| Profiling     | Manual profiling     | System-triggered profiling |
| CPU/GPU usage | Not visible to apps  | Headroom APIs added        |
| Display       | Static refresh rate  | Adaptive refresh rate APIs |
| Bootloader    | Vendor specific      | Generic Bootloader         |
| Accessibility | Limited APIs         | Improved APIs              |
| Notifications | Normal notifications | AI summaries               |

---

✅ **One important point for your build & integration work:**

When comparing **Android 15 and Android 16 source code**, changes usually appear in:

```
frameworks/base/
frameworks/native/
system/core/
bionic/
hardware/interfaces/
```

---





                       ** feature that in aosp 16 but not in aosp 15**


Yes. Sometimes **AOSP introduces completely new features/components that did not exist at all in earlier versions**.
One clear example related to newer Android versions is the **Android Virtualization Framework (AVF)** becoming a **proper system feature in newer AOSP versions (like AOSP 16)**.

I will explain it in a **simple way**.

---

# 1. Android Virtualization Framework (AVF)

## What it is

**Android Virtualization Framework** allows Android to run **virtual machines (VMs)** securely inside the device.

This means Android can run another **isolated operating system environment** inside itself.

Example:

```
Android System
   ↓
Virtual Machine
   ↓
Secure app / Linux environment
```

---

# Before (Older AOSP versions like 15)

There was **no full virtualization framework available for apps**.

Only the system or special environments could use virtualization.

Example limitations:

* Apps could not run **isolated virtual environments**
* Security features were limited
* Hard to run **secure workloads**

So the structure was simply:

```
App
↓
Android Framework
↓
Kernel
↓
Hardware
```

---

# After (AOSP 16)

Android added **full virtualization support using AVF**.

Now Android can run **protected virtual machines (pVM)**.

New flow:

```
App
↓
Virtualization Framework
↓
Hypervisor
↓
Virtual Machine
↓
Secure environment
```

---

# Why Google introduced this

Main reasons:

### 1. Security

Sensitive tasks can run inside a **secure VM**.

Example:

* Digital wallets
* DRM systems
* Secure enterprise apps

Even if Android is compromised, the VM remains protected.

---

### 2. Isolation

Apps or services can run in **isolated environments**.

Example:

```
Main Android System
       ↓
Secure VM (isolated)
```

This prevents malware from accessing secure processes.

---

### 3. Running Linux environments

Developers can run **Linux workloads** inside Android.

Example:

```
Android
↓
Linux VM
↓
Developer tools
```

---

# Where it exists in AOSP source

New directories related to virtualization appear in newer AOSP versions:

```
packages/modules/Virtualization
frameworks/avf
system/virtualization
```

Important components:

```
VirtualizationService
VirtualMachineManager
pVM (Protected Virtual Machine)
```

---

# Real Example Use Case

Example: **Secure payment system**

Before:

```
Payment app
↓
Android OS
↓
Hardware
```

If OS compromised → payment data at risk.

---

After (with virtualization):

```
Payment App
↓
Virtualization Framework
↓
Secure VM
↓
Payment service
```

Even if Android is attacked, the **secure VM protects sensitive data**.

---

                                  **Commits**
---

# 1️⃣ What is a Commit?

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

# 2️⃣ Simple Example

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

# 3️⃣ Structure of a Commit

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

# 4️⃣ Example of an AOSP Commit

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

# 5️⃣ Why Commits Are Important

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

# 6️⃣ Commit Process (Step by Step)

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

# 7️⃣ Example of Multiple Commits

Project history might look like this:

```
Commit 1: Initial framework setup
Commit 2: Add camera service
Commit 3: Fix memory leak
Commit 4: Add CPU headroom API
```

Each commit is a **step in development**.

---

# 8️⃣ How Commits Help in Android Version Comparison

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

# 9️⃣ Backporting Using Commits

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

# 🔟 Real Example of Backporting

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

# Simple Analogy

Think of commits like **saving versions of a document**.

Example:

```
Document v1 → first commit
Document v2 → second commit
Document v3 → third commit
```

You can always **go back to any version**.

---


                                   **millions of commits** 
---

# 1️⃣ How Many Commits Android Has

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

# 2️⃣ What Types of Commits Exist

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

# 3️⃣ How Commits Work Internally

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

# 4️⃣ How Commits Become New Android Versions

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

# 5️⃣ Safe vs Unsafe Commits (Important in Build & Integration)

When integrating commits, engineers classify them.

---

## Safe Commits

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

## Risky / Unsafe Commits

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

# 6️⃣ How Integration Engineers Choose Commits

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

# 7️⃣ Example in Android Build Work

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

# 8️⃣ Simple Way to Remember

You can think of commits like **versions of a book**.

```
Version 1 → commit
Version 2 → commit
Version 3 → commit
```

Android development history is basically **a long chain of commits**.

---


                                                            **types of commits** 
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

# Safe vs Risky for Integration (AOSP → Downstream)

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

# Example Safe Cherry-Pick Candidates

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


