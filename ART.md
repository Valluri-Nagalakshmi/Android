
# ART (Android Runtime) Layer in Android Architecture:

ART (Android Runtime) is the core runtime environment of the Android operating system, responsible for executing Android apps.
The ART layer sits between the Application Framework and the Linux Kernel layers.

# Dalvik Virtual Machine (DVM)

## Introduction:
Dalvik Virtual Machine (DVM) was the process virtual machine used by Android OS to execute Android applications in earlier versions of Android (up to Android 4.x).

## Purpose & Design:
Specialized Virtual Machine (VM) designed for mobile devices.

## Virtual Machine (VM):
A Virtual Machine (VM) is a software-based emulation of a physical computer that provides an isolated environment for executing programs.
It mimics the behavior of actual hardware, allowing multiple OS environments or programs to run on the same hardware independently.

## Types of VMs:
1. System VM: Emulates an entire machine (like VirtualBox, VMware).
2. Process VM: Runs a single process or program, such as:
   - Java Virtual Machine (JVM) → For Java apps.
   - Dalvik Virtual Machine (DVM) → For Android.
   - Android Runtime (ART) → For Android (Newer versions).

## JVM

• JVM (Java Virtual Machine) is designed for desktops & servers, not for mobile devices.  
• JVM is stack-based, which requires more memory and processing power → Bad for mobile devices with low RAM and limited battery.  
• Android needs a runtime that is:
  - Lightweight.
  - Optimized for low memory and low power.
  - Suitable for mobile hardware constraints.
---
![JVM](/images/JVM.png)

---

## DVM/ART:
• DVM/ART are designed for resource-constrained mobile environments:
  - Minimal memory.
  - Low battery usage.
  - Faster start time.
• JVM consumes more memory and drains battery faster on mobile.

## Limitations of JVM for Android:
• JVM lacks Android Framework APIs like:
  - android.app.Activity
  - android.view.View
  - android.os.Bundle  
• Even if JVM could run bytecode, your app would crash without Android framework classes.  
• JVM cannot directly run .apk or .dex files.

### If You Try to Run Android Apps on JVM:
• App won’t even install or execute.  
• Compilation will fail because JVM can’t handle .dex and Android APIs.

---
![JVM vs DVM](/images/JVMvsDVM.jpg)

---

## DVM Focuses on:
• Optimizing memory usage (low RAM footprint).  
• Improving battery life.  
• Good performance on mobile hardware.

## Functionality:
• DVM takes Java source code, compiles it into Java bytecode and then converts it into Dalvik Executable (DEX) format.  
• DEX files are optimized for mobile devices with limited resources.

## Working of DVM:
1. Developers write code in Java.
2. The Java code is compiled using javac to produce Java bytecode (.class files).
3. These .class files are converted to DEX format using the DEX compiler.
4. DVM then loads these DEX files at runtime.
5. Uses Just-In-Time (JIT) Compilation to translate DEX bytecode into machine code during runtime.
6. The translated machine code is executed by the device’s processor.

## Execution Process:
• At runtime, DVM translates DEX code into native machine code using JIT.  
• DVM is register-based unlike JVM which is stack-based.  
• Register-based designs are more suitable for mobile devices with limited resources.  
• DVM’s register-based design helps in better optimization for resource-constrained devices.

## 1. Stack-Based Virtual Machine (e.g., JVM)

### How it Works:
• Uses a stack (like a pile of plates) to hold operands (data values) during computation.  
• Data is pushed (added) onto the stack and popped (removed) from it to perform operations.  
• Instructions don’t include operand addresses; they just say "pop from stack" and "push result."

#### Example:
Let’s say we want to compute:
```
a + b
```
In stack-based VM:
1. Push value of a onto the stack.
2. Push value of b onto the stack.
3. Add → Pops a and b from the stack, adds them, and pushes result back onto the stack.

#### Advantages:
• Simple instruction set (easy to implement).  
• Portable (less dependency on hardware).

#### Disadvantages:
• Slower because of constant stack manipulation (push/pop).  
• More instructions required to complete tasks.

## 2. Register-Based Virtual Machine (e.g., DVM)

### How it Works:
• Uses a set of virtual registers (like CPU registers) to hold data.  
• Operands are stored in registers; instructions directly specify registers for operations (like physical CPU).  
• Fewer memory accesses and reduced stack operations.

#### Example:
Same operation a + b:
1. Load a into Register R1.
2. Load b into Register R2.
3. Add contents of R1 and R2 and store result in Register R3.

#### Advantages:
• Fewer instructions for the same task → Faster execution.  
• Better for performance on limited-resource devices (like mobile phones).

#### Disadvantages:
• Slightly more complex instruction set.  
• Requires more registers (may increase code size slightly).

#### Example DVM Bytecode (Register-Based):
```
add-int v3, v1, v2   // v3 = v1 + v2 (Registers act like variables)
```
## Memory Management:
• DVM is lightweight and efficient in memory usage.  
• Specifically designed to run on devices with limited RAM.

---

## Compilation & Execution Flow of DVM:
```

┌────────────────────────────────────────────┐
│ Java/Kotlin Source                         │
│ ← Developer Writes Code                    │
└───────────────────────┬────────────────────┘
                        ↓ Compilation (javac / kotlinc)            
┌────────────────────────────────────────────┐
│ Java Bytecode (.class)                     │
└───────────────────────┬────────────────────┘
                        ↓ Conversion (dx Tool)                      
┌────────────────────────────────────────────┐
│ Dalvik Bytecode (.dex)                     │
└───────────────────────┬────────────────────┘
                        ↓  Packaging 
┌────────────────────────────────────────────┐
│ APK (Android Package)                      │
└───────────────────────┬────────────────────┘
                        ↓
┌────────────────────────────────────────────┐
│ App Installed with .dex                    │
└───────────────────────┬────────────────────┘
                        ↓ App Launch                               
┌───────────────────────────────────────────────────────────┐
│ DVM Loads .dex Bytecode                                   │
│ → Bytecode is Interpreted                                 │
│ → JIT Compiled (.dex → Machine Code)                      │
│ → App Runs in DVM Process (Isolated)                      │
└───────────────────────────────────────────────────────────┘


```
---
![DVM](/images/DVM.png)

---

## What is an APK?
• APK (Android Package) is the official file format for distributing and installing apps on Android.  
• An APK is simply a compressed package (like a .zip file) containing:
  - Dalvik Bytecode (.dex files) → The compiled app code.
  - Resources → Images, layouts, etc.
  - Manifest → App permissions & metadata.
  - Signatures → Cryptographic signatures to verify app’s authenticity.

#### Potential Risk Factors (Only If You Ignore Security Protections):
APK files can be risky if:
• Installed from untrusted sources (outside Play Store) without verifying the app’s safety.  
• The user manually disables security protections (e.g., turning off Play Protect).  
• Device is rooted or running an unofficial Android version.

---

# ART (Android Runtime)

## Introduction:
• ART (Android Runtime) replaced DVM starting from Android 5.0 (Lollipop).  
• It is now the default runtime environment for Android apps.

## Key Features:
• Supports both:
  - Ahead-Of-Time (AOT) Compilation:
    - Compiles apps into native machine code at installation time.
  - Just-In-Time (JIT) Compilation:
    - Further optimizes app code at runtime.
• Improved garbage collection.
• Enhanced debugging and profiling tools.

## Working of ART:
1. Developer writes code in Java/Kotlin.
2. Code compiled to Java bytecode (.class).
3. Java bytecode converted to DEX bytecode.
4. App packaged as an APK.
5. During app installation:
   - ART performs AOT Compilation to produce native machine code.
   - Generates optimized files.
6. On app launch:
   - ART loads precompiled native code for faster execution.
7. JIT compilation still runs for some additional runtime optimizations.
8. ART includes advanced garbage collection to manage memory effectively.

## Debugging & Profiling:
• ART provides better tools:
  - Sampling profilers.
  - Watchpoint setting for advanced debugging.

## Compilation & Execution Flow:
```

┌───────────────────────────────┐
│ Java/Kotlin Source            │
│ ← Developer Writes Code       │
└───────────────┬───────────────┘
                ↓ Compilation (javac / kotlinc) 
┌───────────────────────────────┐
│ Java Bytecode (.class)        │
└───────────────┬───────────────┘
                ↓ Conversion (dx Tools)     
┌───────────────────────────────┐
│ Dalvik Bytecode (.dex)        │
└───────────────┬───────────────┘
                ↓ Packaging  
┌───────────────────────────────┐                  
│ APK (Android Package)         │
└───────────────┬───────────────┘
                ↓ App Installation (AOT Tool)
┌───────────────────────────────┐
│ AOT Compilation               │
│ (Compiles .dex to             │
│ Native Machine Code → .oat)   │
└───────────────┬───────────────┘
                ↓ .oat file
┌───────────────────────────────┐
│ → Optimized File (.oat) Ready │
└───────────────┬───────────────┘
                ↓  App Launch   
┌─────────────────────────────────────────────────────┐
│ ART Loads Precompiled Native Code                   │
│ → Generally, App Starts Quickly (Precompiled)       │
│ → JIT Compiler Runs If Needed                       │
│ → Garbage Collector Manages Memory                  │
└─────────────────────────────────────────────────────┘


```
---
![ART](/images/ART.png)

---
# Why we are going for ART over DVM

## 1. Faster App Startup
- ART with AOT allows apps to start much faster because the code doesn’t need to be interpreted or JIT compiled at runtime.
- In DVM:
  - App starts slowly because it interprets or JIT-compiles code on every launch.
  - Startup delay happens every time.

## 2. Better Runtime Performance
- Native machine code (from AOT) runs directly on the device’s CPU → much faster.
- DVM needs to JIT compile code repeatedly, causing CPU delay.

## 3. Lower Battery Usage
- AOT compiles code once during install, avoiding CPU-intensive operations during app usage.
- DVM uses CPU repeatedly for JIT at every launch and during app execution → drains battery faster.

## 4. Less CPU Load During App Run
- Since code is precompiled, apps don’t require CPU cycles for code translation during use.
- Frees up CPU for other tasks → smoother multitasking.

## 5. Smoother App Behavior
- ART avoids random lags caused by JIT compilation pauses during app execution.
- DVM could cause noticeable lags when it JIT-compiles code during runtime.

## 6. Better Memory Management
- Precompiled code is more memory-efficient than interpreted or JIT-compiled code.
- ART also optimizes memory layout of apps during AOT.

## 7. Improved Long-Term Performance
- ART also combines AOT with Profile-Guided JIT for hot code.
- Frequently used code paths can be re-optimized based on real usage.

---

# Why AOT Makes Apps Start Faster (Internal Reasoning):

## In DVM / JIT:
1. App’s bytecode (.dex) is interpreted or compiled at runtime.
2. Every time you launch the app, DVM needs to:
   - Parse the bytecode.
Parsing means reading, analyzing, and understanding the structure and contents of a file to process it correctly.
Parsing is mandatory before the code can be executed.
   - DVM must load and understand the entire .dex file to execute it.
   - This step consumes CPU and memory.
   - Compile parts of the code (JIT).
   - This process repeats every time the app runs.
3. This causes:
   - Slower app start (compilation + interpretation needed).
   - Higher CPU usage at runtime.
DVM does not generate .oat files. It directly loads and runs .dex files every time the app is launched.


## In ART with AOT:
1. During app installation:
   - ART compiles the entire app’s bytecode into native machine code.
   - Stores it as an .oat file (Optimized Android file).
2. When you launch the app:
   - The app directly loads this already-compiled machine code.
   - No need for parsing, interpreting, or compiling again.
3. Result:
   - App starts almost instantly.
   - Less CPU processing at launch.
---
![DVM vs ART](/images/DVMvsART.jpg)

---

# DVM vs ART: Full Comparison Table

| Feature                         | **DVM (Dalvik Virtual Machine)**                                         | **ART (Android Runtime)**                                                      |
| ------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| **Runtime Used In**             | Android 2.1 – 4.4                                                        | Android 5.0 (Lollipop) onwards                                                 |
| **Execution Model**             | **Interpretation + JIT** (Just-In-Time)                                  | **AOT (Ahead-of-Time)** + JIT (optional for hot code)                          |
| **Bytecode Format**             | `.dex` (Dalvik Executable)                                               | `.dex` (converted to `.oat`)                                                   |
| **Precompiled Format**          | `.odex` (Optimized DEX) – optimized DEX, not native                      | `.oat` (Optimized Android file) – contains **native machine code**             |
| **Interpretation**              | Bytecode is interpreted line-by-line initially                           | Rarely interpreted; mostly directly executed                                   |
| **Compilation Time**            | Compilation happens during runtime (JIT)                                 | Compilation happens during install (AOT)                                       |
| **App Launch Time**             | Slower (due to JIT every time)                                           | Faster (native code already available)                                         |
| **Battery Usage**               | Higher (JIT consumes CPU repeatedly)                                     | Lower (AOT reduces runtime CPU usage)                                          |
| **CPU Load During App Run**     | High – JIT keeps compiling                                               | Low – precompiled code runs directly                                           |
| **Memory Usage**                | Lower install-time memory usage, but higher runtime memory for JIT cache | Higher install-time storage use (due to .oat), but optimized memory at runtime |
| **Startup Delay**               | Present – caused by JIT and interpretation                               | Minimal – AOT makes apps launch quickly                                        |
| **Code Optimization**           | Limited (only at runtime)                                                | Extensive – AOT + profile-guided JIT for hot code                              |
| **Garbage Collection (GC)**     | Pause-the-world GC, less optimized                                       | Concurrent, compacting GC with better pause times                              |
| **Multi-threading Support**     | Basic                                                                    | Improved GC and thread handling                                                |
| **Exception Handling**          | Slower (because interpreted)                                             | Faster (native execution)                                                      |
| **Debugging Tools**             | Limited                                                                  | Advanced (watchpoints, sampling profilers)                                     |
| **Trace/Profiling**             | Less granular                                                            | More accurate profiling using runtime metrics                                  |
| **File Size of App (APK)**      | Smaller APK (only .dex inside)                                           | Larger total footprint (APK + .oat + .vdex)                                    |
| **OTA Update Speed**            | Faster – no recompilation needed                                         | Slower – requires recompiling updated apps                                     |
| **Shared Code Across Apps**     | `.odex` can be shared                                                    | `.oat` files are usually app-specific                                          |
| **Hot Code Optimization**       | Only JIT                                                                 | JIT + Profile-Guided AOT                                                       |
| **Boot Time Optimization**      | No                                                                       | Boot ClassPath precompiled for faster boot                                     |
| **Security (Code Obfuscation)** | Easy to reverse engineer `.dex`                                          | Harder to reverse engineer `.oat`                                              |
| **Backward Compatibility**      | Not compatible with ART                                                  | ART can fall back to DVM-style JIT in edge cases                               |
| **DEX Cache Use**               | Uses Dalvik cache                                                        | Uses VDEX (Verified DEX) + OAT                                                 |
| **Multidex Support**            | Limited and tricky                                                       | Native support in ART                                                          |
| **Underlying Design**           | Register-based VM                                                        | Native AOT-based + JIT + Garbage Collector                                     |
| **System Impact**               | Higher CPU usage at runtime                                              | Higher disk usage at install, better runtime performance                       |

---

# Clarification of Key Differences

### 1. **.dex vs .odex vs .oat**

| File    | Runtime | Purpose           | Contains                          | Execution                |
| ------- | ------- | ----------------- | --------------------------------- | ------------------------ |
| `.dex`  | DVM/ART | Bytecode          | Dalvik bytecode                   | Needs interpretation/JIT |
| `.odex` | DVM     | Optimized `.dex`  | Still bytecode (organized better) | Needs interpretation/JIT |
| `.oat`  | ART     | AOT compiled code | Native machine code               | Direct CPU execution     |

---

### 2. **JIT vs AOT**

| JIT (DVM)                         | AOT (ART)                             |
| --------------------------------- | ------------------------------------- |
| Happens every time app runs       | Happens only once at install          |
| Slows app launch                  | Makes app launch fast                 |
| Uses battery                      | Conserves CPU and battery             |
| Produces no permanent native code | Produces `.oat` file with native code |

---

### 3. **Execution Path Comparison**

#### In DVM:

* Launch App → Load `.dex` or `.odex` → Interpret + JIT → Execute
* Every launch involves CPU and RAM usage

#### In ART:

* Install App → Convert `.dex` to `.oat` (native) → Store it
* Launch App → Directly load `.oat` → Fast execution with low overhead

---

# Why ART Replaced DVM

| Need            | Why DVM Failed                | How ART Fixed It                  |
| --------------- | ----------------------------- | --------------------------------- |
| Performance     | Slow launches, high JIT load  | Native code ready via AOT         |
| Battery         | High CPU usage during runtime | Minimal CPU load                  |
| Memory          | High JIT cache usage          | Compact layout + GC optimizations |
| Developer Needs | Weak profiling/debug tools    | Advanced tooling support          |
| User Experience | Lags, pauses                  | Smooth transitions                |

---

