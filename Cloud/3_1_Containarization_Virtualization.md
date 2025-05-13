# Demystifying Containers - Part I: Kernel Space (Medium Article)

This article, the first in a series, provides a pragmatic view on containers from a historic perspective, starting at the Linux Kernel level.

## Introduction
- Containers are often mistakenly thought of as lightweight virtual machines.
- They are fundamentally isolated groups of processes on a single host.
- Four major requirements for containers: run on a single host, are groups of processes (with a root), need isolation, and must fulfill common features.
- The concept is explored from a historical perspective.

## chroot
- Introduced in UNIX Version 7 (1979), later adopted in BSD and Linux.
- Allows changing the root directory of a process and its children.
- Also known as "jail," used historically for security purposes (like honeypots) and early "microservices."
- Basic usage involves creating a new root directory, copying necessary binaries/libraries, and running `chroot`.
- A simple `chroot` jail is limited and not a strong security feature on its own, as a root user can easily escape.
- Modern container runtimes use `pivot_root(2)` instead of `chroot(2)`, which provides better isolation of old mount points.
- Discusses obtaining a usable root filesystem (rootfs) from an OCI container image using `skopeo` and `umoci` for use with `chroot`.
- Demonstrates that `chroot` alone does not provide process or network isolation, highlighting its limitations for security.

## Linux Namespaces
- Introduced in the Linux kernel starting in 2002 (Linux 2.4.19).
- Wrap global system resources in an abstraction layer to give processes an isolated view.
- Full "container ready" support achieved in kernel 3.8 (2013) with the `user` namespace.
- Currently, seven distinct namespaces exist: `mnt`, `pid`, `net`, `ipc`, `uts`, `user`, and `cgroup`. (Mentions proposed `time` and `syslog` namespaces).

### API
- The kernel namespace API consists of three main system calls:
    - `clone(2)`: Creates a new child process, allowing shared execution context or new namespaces via flags.
    - `unshare(2)`: Allows a process to disassociate from shared execution context, creating new namespaces for itself.
    - `setns(2)`: Reassociates the calling thread with a given namespace file descriptor, allowing joining an existing namespace.
- The `/proc` filesystem provides magic links (`/proc/$PID/ns/*`) that act as handles to namespaces for use with `setns(2)`.
- Tools from the `util-linux` package (like `lsns`) provide command-line wrappers for namespace operations.

### Available Namespaces
- **Mount (mnt):** First implemented (2002). Isolates mount points. Used to create secure jail-like environments. Demonstrates creating and using a separate mount space with `unshare -m` and `tmpfs`, showing it's isolated from the host. Mentions `mountinfo` in `/proc` for viewing mounts and touches on mount flavors (shared, slave, private, unbindable).
- **UNIX Time-sharing System (uts):** Introduced in Linux 2.6.19 (2006). Isolates the domain name and hostname. Demonstrates changing the hostname within a namespace using `unshare -u`.
- **Interprocess Communication (ipc):** Introduced in Linux 2.6.19 (2006). Isolates IPC resources (System V IPC objects, POSIX message queues) to prevent unintended sharing. IPC objects are destroyed when the namespace is destroyed.
- **Process ID (pid):** Introduced in Linux 2.6.24 (2008). Gives processes independent sets of PIDs, allowing processes in different namespaces to have the same PID. Namespaces can be nested. The first process gets PID 1 and special init-like behavior (re-parenting, termination). Demonstrates using `unshare -fp --mount-proc` to create an isolated PID view. `/proc/$PID/ns/pid` links to the namespace.
- **Network (net):** Completed in Linux 2.6.29 (2009). Virtualizes the network stack. Each namespace has its own resources (`/proc/net`), starting with only a loopback interface. Demonstrates creating a namespace with `unshare -n` and managing it using `ip netns add` and `ip netns exec`. Shows how to set up communication between namespaces and the host using `veth` pairs.
- **User ID (user):** Isolation introduced in Linux 3.5 (2012), unprivileged creation in 3.8 (2013). Isolates user and group IDs, allowing a process to be unprivileged outside but privileged inside the namespace. Demonstrates using `unshare -U`. `/proc/$PID/{u,g}id_map` files control mappings. Discusses the `setgroups` security issue and the `/proc/$PID/setgroups` file. Essential for rootless containers.
- **Control Group (cgroup):** Feature started in 2008 (Linux 2.6.24), namespace added in 4.6 (2016). Used for resource limiting, prioritization, accounting, and controlling. Cgroup v2 (2013) introduced major redesigns. Demonstrates creating a cgroup directory in `/sys/fs/cgroup`, setting memory limits (`memory.limit_in_bytes`, `memory.swappiness`), and assigning processes (`cgroup.procs`). Shows a Rust example that gets killed by memory limits within a cgroup.

## Composing Namespaces
- Namespaces are composable, enabling complex isolation scenarios like Kubernetes Pods (isolated PIDs sharing a network interface).
- Demonstrates using `nsenter` with `/proc/$PID/ns/pid` to join an existing PID namespace.

## Demo Application
- Provides C code for a simple application that uses the `clone(2)` syscall with various `CLONE_NEW*` flags to create a child process in new mount, network, UTS, IPC, PID, and user namespaces.
- Shows how to compile and run the demo to verify isolation for commands like `ip a`, `ps aux`, and `whoami`.
- Explains this is a basic example to show how runtimes leverage namespaces.

## Putting it all Together
- Briefly mentions using a low-level container runtime like `runc` with a rootfs (extracted earlier with `skopeo`/`umoci`).
- Shows using `lsns` to see the namespaces (`mnt`, `uts`, `ipc`, `pid`, `net`) created by `runc`.

## Conclusion
- Summarizes that containers use various Linux kernel isolation features (like `chroot`, `pivot_root`, and especially namespaces) at different abstraction levels.
- Mentions the Linux programmers manual (`NAMESPACES(7)`) as a resource for deeper study.
- Previews upcoming posts on container runtimes, security, and the ecosystem.

# Kernel Korner - Unionfs: Bringing Filesystems Together (Linux Journal Article)

This article from Linux Journal's "Kernel Korner" series explains Unionfs, a filesystem that allows merging the contents of several directories or filesystems into a single, unified view.

## Introduction to Unionfs
- Unionfs enables keeping related files in separate physical locations while presenting them together logically.
- A collection of merged directories is called a union, and each physical directory is a branch.
- Unionfs layers on top of multiple filesystems or directories within the same filesystem, a technique known as stacking.
- It acts as a filesystem interface to the kernel and presents itself as the kernel's VFS to the filesystems it stacks on.
- Unionfs is a true "fan-out" filesystem, capable of directly accessing many underlying branches.

## Unionfs Semantics and Usage
- Each branch in Unionfs is assigned a precedence; higher precedence overrides lower precedence.
- When directories exist in multiple branches, the Unionfs directory combines their contents and attributes, removing duplicates.
- If a file exists in multiple branches, the version in the higher-priority branch is used, and lower-priority versions are ignored.
- Demonstrates how to mount Unionfs using the `mount` command, specifying branches with the `dirs` option. The example merges `/Fruits` and `/Vegetables` into `/mnt/healthy`, showing how precedence affects which file version is visible.
- Shows recursive merging of subdirectories.
- Mentions various applications, including unifying home directories, merging ISO images, source code management, and snapshotting with copy-on-write.

## Unified Home Directories
- Explains how Unionfs can merge home directories from multiple NFS servers into a single mount point (e.g., `/home`), avoiding the need for symbolic links used by some automounters.
- Notes that Unionfs supports multiple read-write branches, preventing files from migrating between directories, unlike older systems.

## Merging Distribution ISO Images
- Describes the problem of needing both ISO images (for burning CDs) and individual packages (for network installs) of Linux distributions, leading to wasted space and effort.
- Unionfs can merge the contents of mounted ISO images (using loopback devices) from separate directories (e.g., `/mnt/disc1`, `/mnt/disc2`) into a single, unified view (e.g., `/mnt/merged-distribution`) suitable for network installation via NFS or FTP.

## Copy-on-Write Unions
- Unionfs can mix read-only and read-write branches, making the union read-write overall.
- Uses copy-on-write semantics: modifications to files on read-only branches are copied up to a higher-priority read-write branch. Demonstrates patching a CD-ROM by mounting it read-only below a read-write temporary directory.
- Explains the `copyup` operation when a read-only file is opened for writing.
- Shows how to explicitly mark a read-write branch as read-only within the union using `=ro` in the `dirs` mount option, useful for source code versioning with pristine sources.
- Discusses using an overlay mount to replace the original directory with the unified view.

## Implementation
- Most filesystem operations traverse from higher-priority branches down to lower ones.
- `LOOKUP` starts high and moves down, caching information.
- `CREATE` attempts creation in the highest-priority parent branch, moving down if needed. Error handling proceeds from lower to higher priority.
- `UNLINK` (delete) operations proceed from lower to higher branches.
- Unionfs uses "whiteouts" (zero-length files named `.wh.FILENAME`) in higher branches to make files in lower branches appear deleted, even if the lower branch is read-only. Whiteouts help maintain UNIX semantics and handle partial errors.

## File Deletion Semantics
- Default deletion mode is `DELETE_ALL`, attempting to delete all instances in all branches.
- `DELETE_WHITEOUT`: Creates a whiteout instead of deleting the lower files, keeping them accessible via the underlying filesystem.
- `DELETE_FIRST`: Departs from standard UNIX semantics by only removing the highest-priority entry, allowing a lower-priority file to show through. This mode is useful for source code versioning to revert changes. Deletion mode is set via a mount option (`delete=first`).

## Unionfs Snapshots
- The `unionctl` utility allows changing branch configuration dynamically (adding/removing branches, changing read-only/read-write status).
- Demonstrates creating a snapshot by mounting Unionfs with a single read-write branch, then adding a new branch (`/snaps/0`) for snapshot files. Explains where new files are physically created based on parent directory existence and branch precedence.
- Completing the snapshot involves marking the original branch read-only (`unionctl /usr --mode /usr ro`).
- Multiple snapshots can be created incrementally.
- Snapshots can be viewed by mounting Unionfs with the snapshot directories layered over the base directory.
- The `snapmerge` script is used to merge snapshot changes (new/changed files and deletions via whiteouts) back into the base directory.

## Conclusion
- Unionfs merges multiple branches into a single virtual view using an efficient fan-out structure.
- It is suitable for various applications like merged ISOs, unified home directories, source versioning, snapshotting, and patching CD-ROMs using copy-on-write semantics.
- Performance benchmarks show low overhead (10-12%) for compile and I/O workloads on Linux 2.4.

## Acknowledgements
- Mentions other members of the Unionfs development team.
- Provides a link to additional resources.

## Authors
- Charles P. Wright and Erez Zadok, detailing their roles and research focus.

# Enabling Technologies: Virtualization and Containerization

## Virtualization

Virtualization allows running multiple operating systems or applications on a single physical machine[cite: 1]. It creates a virtualization layer between the guest (virtual image, applications) and the host (physical hardware, storage, networking)[cite: 2].

### Characteristics of Virtualization
- **Increased security**: The virtual machine manager (VMM) or hypervisor controls and filters guest activity, preventing harmful operations. Resources can be hidden or protected. Sensitive host information can be hidden without complex security policies[cite: 3]. Examples include sandboxed Java Virtual Machines (JVM) [cite: 3, 4] and completely separated file systems in hardware virtualization[cite: 3].
- **Managed execution**: Provides capabilities like sharing, aggregation, emulation, and isolation of resources[cite: 5].
- **Portability**: Allows transferring data and applications between computing platforms[cite: 6]. Virtualization increases portability because VMs are booted from VM images stored in specific disk formats (e.g., aki, ari, ami, vdi, vhd, vhdx, vmdk) that can be moved or converted[cite: 9, 10]. Docker containers can run on any platform with the Docker engine[cite: 8].

### Machine Reference Model
- Computer systems have a layered structure: Hardware, Instruction Set Architecture (ISA), Operating System, Application Binary Interface (ABI), Libraries, and Applications[cite: 11].
- Applications make API calls to Libraries, which make system calls to the Operating System[cite: 11]. The Operating System interacts with Hardware via the ISA[cite: 11].

### Privileged and Nonprivileged Instructions
- Hardware operates in different modes or rings (Ring 0 to Ring 3)[cite: 8].
    - Ring 0: Most privileged (Supervisor/Kernel mode)[cite: 8].
    - Ring 3: Least privileged (User mode)[cite: 8].
    - Rings 1 and 2 are not used in modern systems[cite: 8].
- **Nonprivileged instructions** can be used without interfering with other tasks (e.g., arithmetic, floating-point)[cite: 12].
- **Privileged instructions** have restrictions and are for sensitive operations (e.g., I/O, changing CPU state)[cite: 13].

### Hypervisor (Virtual Machine Monitor - VMM)
- The hypervisor runs in supervisor (kernel) mode[cite: 9, 14].
- The challenge is to emulate/manage the CPU state for guest OS, requiring sensitive instructions to run in privileged mode[cite: 9, 14]. Original ISAs had sensitive instructions executable in user mode, preventing guest OS isolation[cite: 9]. Intel VT and AMD-V redesigned instructions as privileged[cite: 9].
- A VMM must satisfy three properties[cite: 18]:
    - **Equivalence**: Guest behavior should be the same as on physical hardware[cite: 19].
    - **Resource control**: VMM has complete control of virtualized resources[cite: 20].
    - **Efficiency**: Most instructions run without VMM intervention[cite: 21]. (Popek and Goldberg requirements [cite: 22]).

### Hardware Level Virtualization
- VMM runs on bare metal (Type I) or on a host OS (Type II)[cite: 10, 15].

#### Type I Hypervisor (Native VMs)
- Sits on bare metal (directly on hardware)[cite: 10, 13].
- Can be microkernel (e.g., Xen, MS Hyper-V) or monolithic (e.g., VMware ESX)[cite: 13].
    - Microkernel VMMs handle CPU/memory; device drivers are in a privileged guest OS (Domain 0)[cite: 13].
    - Monolithic VMMs handle everything[cite: 13].
- **Xen Architecture Example**: Xen Hypervisor manages memory, CPU state, and I/O. Requires guest OS modification (paravirtualization) and uses hypercalls for sensitive instructions. VM management is done by the Domain 0 OS[cite: 13].
- **Trap on Privileged Instructions**: When a guest OS tries to execute a kernel-only instruction, it traps to the hypervisor (if hardware virtualization is present)[cite: 25, 26, 27]. The hypervisor inspects the instruction: if from guest OS, it performs it; if from user program, it emulates hardware behavior[cite: 27].

#### Type II Hypervisor (Hosted VMs)
- Runs on top of a host operating system[cite: 10, 15].

#### Examples of Hypervisors (Type 1 and Type 2)[cite: 17]:
- **Virtualization without HW support**: ESX Server 1.0 (Type 1), VMware Workstation 1 (Type 2).
- **Paravirtualization**: Xen 1.0 (Type 1), VirtualBox 5.0+ (Type 2).
- **Virtualization with HW support**: vSphere, Xen, Hyper-V (Type 1), VMware Fusion, KVM, Parallels (Type 2).

### Full Virtualization
- The VMM (Hypervisor) scans the instruction stream[cite: 28].
- Noncritical instructions run directly on hardware[cite: 28].
- Critical (privileged, control- and behavior-sensitive) instructions are trapped to the VMM, which emulates their behavior[cite: 28].
- Uses binary translation [cite: 10, 15, 28] or hypercalls[cite: 28].
- Guest OS is unaware of virtualization and its codebase is unmodified[cite: 15, 28].

### Challenges of Virtualization on x86
- Original x86 architecture was not fully virtualizable because sensitive instructions weren't a subset of privileged ones[cite: 29].
- Complexity of the x86 architecture[cite: 29].
- Diverse x86 peripherals[cite: 29].
- Need for a simple user experience[cite: 29].

### Binary Translation
- Involves rewriting binary code before execution and emulating sensitive instructions[cite: 17, 30].
- Guest OS privileged instructions trap to the hypervisor, which performs them after checks[cite: 31, 32].
- Guest OS sensitive instructions trigger the hypervisor to rewrite code blocks, replacing sensitive instructions with hypervisor calls[cite: 31, 33]. Branches are replaced with calls to the hypervisor to continue translation[cite: 33].
- Dynamic binary translation is not overly expensive due to caching[cite: 34]. Most code blocks don't have sensitive/privileged instructions and run natively[cite: 34]. User processes can be ignored as hardware ignores sensitive instructions in user mode[cite: 34, 35]. Guest OS code (Ring 1) is translated to avoid expensive traps[cite: 36].

### Hardware-supported Virtualization Technology
- Intel VT-x (2005) and AMD-v made sensitive instructions a subset of privileged instructions[cite: 37].
- Sensitive instructions now trap to the hypervisor, which handles them[cite: 37]. (Note: Many traps can slow performance compared to binary translation [cite: 37]).

### Paravirtualization
- Guest OS is modified to be aware it's running in a VM[cite: 22, 38, 39].
- Nonvirtualizable instructions are replaced by hypercalls[cite: 38].
- Hypervisor exposes an API that the guest OS uses instead of standard syscalls[cite: 39]. Sensitive instructions are replaced by hypercalls[cite: 39].
- Requires the guest OS to support paravirtualization[cite: 38].
- Often leveraged by Microkernels, which handle core OS functions and interact with the hypervisor[cite: 40, 41, 42].
- Examples: Xen, KVM (Linux), Hyper-V[cite: 17, 38].

## Containerization

Containerization is Operating System-level virtualization[cite: 43, 45]. It leverages multiprogramming techniques at the OS level[cite: 45].

### What is a Container
- Isolated groups of processes running on a single host, sharing the same host OS kernel[cite: 45, 74].
- No virtual machine manager or hardware emulation is used[cite: 45].
- Based on core Linux technologies: Namespaces, Cgroups, and UnionFS[cite: 45].

### Fundamental Technologies for Containers

#### Namespace
- Wraps a global system resource in an abstraction layer[cite: 52].
- Processes within a namespace perceive their own isolated instance of the resource[cite: 52].
- Changes in a namespace are visible to other members but invisible outside the namespace[cite: 53].
- Key namespaces include[cite: 52]:
    - Cgroup: Cgroup root directory
    - IPC: System V IPC, POSIX message queues
    - Network: Network devices, stacks, ports, etc.
    - Mount: Mount points
    - PID: Process IDs
    - User: User and group IDs
    - UTS: Hostname and NIS domain name
- System calls for namespace management:
    - `clone()`: Creates a new process in one or more new namespaces[cite: 54, 55].
    - `setns()`: Joins an existing namespace[cite: 55, 56].
    - `unshare()`: Moves a process's context into a new namespace[cite: 55, 56].
- Containerization evolved from the `chroot` mechanism, which isolates the filesystem root for a process[cite: 51]. `pivot_root(2)` has replaced `chroot` in modern container runtimes[cite: 51].

#### Cgroups (Control Groups)
- Primary goal: Resource limiting, prioritization, accounting, and controlling[cite: 57].
- Organizes processes into hierarchical groups[cite: 57].
- Interface provided through a pseudo-filesystem called `cgroupfs` (`/sys/fs/cgroup`)[cite: 58].
- Allows setting limits (e.g., memory limits via files like `memory.limit_in_bytes`) and assigning processes to groups (`cgroup.procs`)[cite: 60, 61].

#### UnionFS
- Allows logically merging several underlying directories or filesystems (branches) into a single virtual view (a union)[cite: 45, 62, 63].
- Assigns precedence to branches; higher precedence overrides lower[cite: 67].
- Combines directory contents and attributes, removing duplicates[cite: 67].
- If a file exists in multiple branches, the higher-priority version is used[cite: 67].
- Supports mixing read-only and read-write branches, with the union being read-write overall[cite: 69].
- Uses copy-on-write semantics: writes to read-only branches are copied up to a higher-priority read-write branch (a `copyup` operation)[cite: 69, 71, 72].
- Example: Merging /Fruits and /Vegetables directories[cite: 64, 65, 66].

### Application Containers vs System Containers
- **Application containers**:
    - Contain a single process[cite: 49].
    - Use a layered filesystem[cite: 49].
    - Run microservices/applications[cite: 49].
    - Used for distributing applications[cite: 49].
    - Implementations: Docker, CRI-O[cite: 49].
- **System containers**:
    - Contain an entire operating system[cite: 49].
    - Filesystem neutral[cite: 49].
    - Run a lightweight virtual machine[cite: 49].
    - Used for providing underlying infrastructure[cite: 49].
    - Implementations: LXC/LXD, OpenVZ/Virtuozzo, BSD jails, Linux vServer[cite: 49].

### Docker

Docker is a popular containerization platform[cite: 73].

#### Docker Architecture
- Docker engine is a Client/Server application[cite: 74].
- **Client (docker CLI)**: Command line interface to interact with the server[cite: 74].
- **Server (Docker daemon)**: Creates and manages Docker objects (containers, images, networks, data volumes)[cite: 74].
- The CLI uses the Docker REST API to communicate with the daemon[cite: 74].

#### Images and Containers
- **Image**: A read-only template with instructions for creating a container[cite: 75]. They are layered, defined by a Dockerfile[cite: 75, 77].
- **Container**: A runnable instance of an image[cite: 75]. Managed with the CLI or API[cite: 75].
- When running a container (e.g., `docker run -i -t ubuntu /bin/bash`), the daemon locates/downloads the image, creates a new container, allocates a R/W filesystem layer on top of the image, creates a network interface, and starts the command[cite: 76, 77]. The container's R/W filesystem is isolated[cite: 76].

#### Storage Options [cite: 78]
- **Container writable layer**: Does not persist after termination. Tightly coupled with the host, reducing portability. Reduced performance[cite: 79].
- **Volumes**: Created and managed by Docker. Stored in a directory on the host but managed by Docker (isolated access). Can be mounted R/W or R in multiple containers simultaneously. Easier to back up/migrate, safer to share among containers. Volume drivers allow storage on remote/cloud providers, encryption, etc. Content can be pre-populated. Work on Linux and Windows. Managed via CLI or API[cite: 80, 81, 82, 83, 84, 85].
- **Bind mounts**: Mount any host filesystem area (file or directory) into a container, referenced by its host path. Can be created by the container if they don't exist. Shared with host processes. Not portable. Usually higher performance than volumes[cite: 86].
- **Tmpfs mount**: Mounts a temporary memory area outside the container's writable layer. Persisted only in host memory. Removed when the container stops. Available only in Linux hosts. Not sharable among containers[cite: 87].
- **Activity cases for storage options**[cite: 88, 89]:
    - Backing up/migrating data: Volumes.
    - Sharing data among multiple running containers: Volumes.
    - Sharing configuration from host to containers: Bind mount (if host path is guaranteed).
    - Data shouldn't persist: Tmpfs mount.
    - Sharing source code/build artifacts: Bind mount (if host path is guaranteed).
    - Storing data on remote/cloud: Volumes.
    - Host filesystem structure guaranteed: Bind mount.
    - Host filesystem structure NOT guaranteed: Volumes.

#### Networking [cite: 90]
- Containers can connect to each other or non-Docker workloads[cite: 90].
- Containers don't need to be aware they are on Docker[cite: 90].
- Types of network drivers[cite: 90]:
    - **Bridge**: Default driver. For standalone containers communicating with each other[cite: 91].
    - **Host**: Removes network isolation, container uses host networking directly. For standalone containers[cite: 91].
    - **Overlay**: Connects multiple Docker daemons for swarm services or standalone containers on different hosts to communicate[cite: 92, 93].
    - **Macvlan**: Assigns a MAC address to a container, making it appear as a physical device on the network. Useful for legacy applications expecting direct physical network connection[cite: 94, 95].
    - **None**: Disables networking for the container[cite: 95].
- **Activity cases for networking drivers**[cite: 96, 97, 98]:
    - Migrating from VM setup: Macvlan.
    - Network stack not isolated: Host networks.
    - Containers on different hosts communicate: Overlay networks.
    - Multiple containers on same host communicate: User-defined bridge networks.
    - Containers look like physical hosts (unique MAC): Macvlan networks.

### Outcome
- Covered Docker and its underlying technologies: Namespace, Cgroup, Union FS[cite: 99].
- Explained roles of Docker daemon, CLI, and API[cite: 99].
- Discussed Layered images[cite: 99].
- Reviewed Storage options: volume, bind, tmpfs[cite: 99].
- Outlined Networking concepts and drivers[cite: 99].

