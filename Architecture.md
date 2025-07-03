
#  Linux Architecture:

![Linux Architecture](/images/linux_architecture.jpeg)

---

##  **1. Applications**

Programs that users directly interact with.
Applications are user programs that perform tasks for the user. They run in **User Space** (outside the kernel).

###  **Examples:**

- Firefox: Web browser  
- VLC Player: Media Player  
 

###  **Where It Runs:**

User Space → Uses system calls to request Kernel services.

---

##  **2. Services (Daemons)**

Background processes that run continuously and provide core services.
These are **system services** (often called **daemons**) that start at boot time or on demand and provide features to applications and users.

###  **Examples:**

- sshd: Secure Shell (SSH) remote login service  
- systemd: Initializes and manages system services 
- udevd:  Manages device nodes in /dev

###  **Where It Runs:**

User Space (Background processes), interacts with Kernel & Hardware via system calls.

---

##  **3. Libraries**

Shared code that applications and services use.
Libraries offer **pre-written functions** so programs don’t need to rewrite common functionalities.

###  **Examples:**

- glibc: Standard C Library (memory, math, string functions)    
- libm: Math Library  
- libpthread: POSIX Threads (multithreading)  

###  **Where It Runs:**

User Space (linked at runtime or compile time).

---

##  **4. Drivers (Kernel Modules)**

Programs that help the OS communicate with hardware devices.
Device drivers are part of the **Kernel**. They provide control and communication with hardware components.

###  **Examples:**
  
- Wi-Fi driver
- USB driver 
- Bluetooth driver

###  **Where It Runs:**

Kernel Space (can be loaded/unloaded as modules).

---

## **5. Kernel**

Core part of the OS; manages hardware, memory, processes, and system calls.
The Kernel is the **heart** of the operating system. It controls everything from process management to memory and hardware communication.

###  **Examples of Kernel Features:**

- Process Management: Task scheduling, process creation (fork)  
- Memory Management: RAM allocation, virtual memory, paging  
- File System: Handling file systems  
- Networking Stack: Managing TCP/IP communication  

###  **Where It Runs:**

Kernel Space (highest privilege level).
```

---


