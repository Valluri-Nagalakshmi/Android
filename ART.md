
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
• DVM is register-based unlike JVM which is stack-based:  
• Register-based designs are more suitable for mobile devices with limited resources.  
• DVM’s register-based design helps in better optimization for resource-constrained devices.

## Memory Management:
• DVM is lightweight and efficient in memory usage.  
• Specifically designed to run on devices with limited RAM.

## Compilation & Execution Flow:
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

## Why DVM Was Used in Android Instead of JVM?
**Reason:**
• JVM (Java Virtual Machine) is designed for desktops & servers, not for mobile devices.  
• JVM is stack-based, which requires more memory and processing power → Bad for mobile devices with low RAM and limited battery.  
• Android needs a runtime that is:
  - Lightweight.
  - Optimized for low memory and low power.
  - Suitable for mobile hardware constraints.

## DVM/ART:
• DVM/ART are designed for resource-constrained mobile environments:
  - Minimal memory.
  - Low battery usage.
  - Faster start time.
• JVM consumes more memory and drains battery faster on mobile.

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

## What is an APK?
• APK (Android Package) is the official file format for distributing and installing apps on Android.  
• An APK is simply a compressed package (like a .zip file) containing:
  - Dalvik Bytecode (.dex files) → The compiled app code.
  - Resources → Images, layouts, etc.
  - Manifest → App permissions & metadata.
  - Signatures → Cryptographic signatures to verify app’s authenticity.

### Why APK ≠ Direct Threat by Default:
| Security Layer  | Protection Mechanism                                    |
|-----------------|---------------------------------------------------------|
| App Signing     | Ensures authenticity; blocks tampered apps.             |
| Sandboxing      | Restricts apps from accessing other apps' data.         |
| Permission Model| Prevents unauthorized access to sensitive data.         |
| Google Play Protect | Scans apps for malware before/after install.        |
| Verified Boot   | Ensures OS integrity; prevents low-level malware.       |

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
                ↓ Conversion (AOT Tools)     
┌───────────────────────────────┐
│ Dalvik Bytecode (.dex)        │
└───────────────┬───────────────┘
                ↓ Packaging  
┌───────────────────────────────┐                  
│ APK (Android Package)         │
└───────────────┬───────────────┘
                ↓ App Installation 
┌───────────────────────────────┐
│ → AOT Compilation             │
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
```

---

