
# ART (Android Runtime) Layer in Android Architecture:

ART is the runtime environment in Android (from version 5.0+), responsible for executing apps. It sits between the Application Framework and Linux Kernel.

# Dalvik Virtual Machine (DVM)

## Introduction:
Used in Android versions up to 4.x to run apps.

## Purpose & Design:
A lightweight VM optimized for mobile devices (low RAM, low battery).

## Virtual Machine (VM):
Software emulation of hardware that runs programs in isolated environments.

## Types of VMs:
1. **System VM**: Emulates full machine (e.g., VirtualBox).
2. **Process VM**: Runs single app (e.g., JVM, DVM, ART).

## DVM Focuses on:
• Low memory usage  
• Battery efficiency  
• Good mobile performance

## Functionality:
Java code → .class (bytecode) → .dex (Dalvik executable) → loaded by DVM → JIT compiled at runtime

## Working of DVM:
1. Java source → .class via `javac`  
2. .class → .dex via dx tool  
3. DVM interprets or JIT compiles .dex at runtime  
4. Native code is executed

## Execution Process:
DVM is **register-based**, better for limited-resource devices (vs stack-based JVM).

## 1. Stack-Based VM (e.g., JVM)

### How it Works:
Uses a stack to manage operands.

#### Example:
```

a + b

```
1. Push a  
2. Push b  
3. Add → pop a, b → push result

#### Pros:
• Simple  
• Portable

#### Cons:
• More instructions  
• Slower due to stack ops

## 2. Register-Based VM (e.g., DVM)

### How it Works:
Uses virtual registers to hold operands directly.

#### Example:
```

a + b

```
1. Load a into R1  
2. Load b into R2  
3. Add → R3 = R1 + R2

#### Pros:
• Fewer instructions  
• Faster on mobiles

#### Cons:
• More complex  
• More registers

#### Example Bytecode:
```

add-int v3, v1, v2

```

## Memory Management:
DVM is memory-efficient, suitable for devices with low RAM.

## Compilation & Execution Flow:
```

Java/Kotlin → .class → .dex → APK → Install → DVM loads .dex → Interpret/JIT → Run

```

---
![DVM](/images/DVM.png)

---

## Why DVM Was Used Instead of JVM?
---
![JVM vs DVM](/images/JVMvsDVM.jpg)

---

• JVM is designed for desktops/servers → high memory & power needs  
• Android needed a lightweight runtime  
• JVM is stack-based → not efficient on mobiles  

---
![JVM](/images/JVM.png)

---

## DVM/ART vs JVM:
• DVM/ART: Designed for low memory & power  
• JVM: High memory usage, lacks Android APIs

## JVM Limitations:
• Can’t run .apk/.dex  
• No access to Android APIs  
• App will crash if run under JVM

### JVM Can’t Run Android Apps:
• Compilation fails  
• Framework classes missing

## What is an APK?
A zip archive containing:
- `.dex` bytecode  
- Resources (layouts, images)  
- Manifest  
- Signatures

#### Risks:
• Unsafe if installed from unknown sources  
• Riskier on rooted/unofficial Android devices

---

# ART (Android Runtime)

## Introduction:
Default runtime from Android 5.0 (Lollipop) onward.

## Key Features:
• AOT + JIT compilation  
• Better GC  
• Improved debugging/profiling

## Working of ART:
1. Java/Kotlin → .class → .dex → APK  
2. During installation → AOT compiles to native (.oat)  
3. On launch → native code loaded  
4. JIT can still optimize runtime code  
5. GC manages memory

## Debugging & Profiling:
• **Sampling profilers**: Collect runtime data periodically  
• **Watchpoints**: Trigger breakpoints on memory access

## Compilation & Execution Flow:
```

Java/Kotlin → .class → .dex → APK → AOT → .oat → Launch → Native execution + JIT + GC

```

---
![ART](/images/ART.png)

---

# Why ART Over DVM?

## 1. Faster Startup:
• Precompiled code → no delay at launch  
• DVM interprets/JITs on each run → slower

## 2. Better Performance:
• AOT-native code runs directly on CPU  
• No JIT delay during critical tasks

## 3. Lower Battery Usage:
• AOT avoids repeated JIT  
• Saves CPU cycles → less power

## 4. Less CPU Load:
• No need to JIT during runtime  
• CPU freed for other tasks

## 5. Smoother UX:
• No runtime pauses due to JIT  
• Better frame rates and responsiveness

## 6. Memory Efficiency:
• Native code layout optimized at install  
• Less runtime memory overhead

## 7. Long-Term Optimization:
• Combines AOT + Profile-Guided JIT  
• Optimizes “hot code paths” based on usage

---

# Why AOT Makes Apps Start Faster:

## In DVM / JIT:
1. At every launch:
   - Parse and interpret .dex  
   - JIT compile code  
   - High CPU + memory use  
2. Slower app startup every time  
3. DVM does **not** create .oat

## In ART:
1. At install:
   - AOT compiles .dex → native (.oat)  
2. At launch:
   - Directly runs .oat (no parsing/interpreting)  
3. Result:
   - Near-instant startup  
   - Low CPU load

---
![DVM vs ART](/images/DVMvsART.jpg)
```

---


