.. _namespc-cgroup:

Namespaces and cgroups
======================

What Are Namespaces?
____________________

Namespaces have been part of the Linux kernel since about 2002, and over time more
tooling and namespace types have been added. Real container support was added to
the Linux kernel only in 2013, however. This is what made namespaces really useful
and brought them to the masses.

.. callout :: Definition according to `Wikipedia <https://en.wikipedia.org/wiki/Linux_namespaces>`_

  `Namespaces are a feature of the Linux kernel that partitions kernel resources
  such that one set of processes sees one set of resources while another set of
  processes sees a different set of resources.`

In other words, the key feature of namespaces is that they isolate processes from
each other. On a server where you are running many different services, isolating
each service and its associated processes from other services means that there is
a smaller blast radius for changes, as well as a smaller footprint for security‑related
concerns.

Using containers during the development process gives the developer an isolated
environment that looks and feels like a complete VM. It’s not a VM, though – it’s
a process running on a server somewhere. If the developer starts two containers,
there are two processes running on a single server somewhere – but they are isolated
from each other.

Types of Namespaces
+++++++++++++++++++

Within the Linux kernel, there are different types of namespaces. Each namespace
has its own unique properties:

- A `user namespace <https://man7.org/linux/man-pages/man7/user_namespaces.7.html>`_
has its own set of user IDs and group IDs for assignment to processes. In particular,
this means that a process can have root privilege within its user namespace without
having it in other user namespaces.

- A `process ID (PID) namespace <https://man7.org/linux/man-pages/man7/pid_namespaces.7.html>`_
assigns a set of PIDs to processes that are independent from the set of PIDs in other namespaces.
The first process created in a new namespace has PID 1 and child processes are assigned subsequent PIDs.
If a child process is created with its own PID namespace, it has PID 1 in that namespace
as well as its PID in the parent process’ namespace. See below for an example.

- A `network namespace <https://man7.org/linux/man-pages/man7/network_namespaces.7.html>`_
has an independent network stack: its own private routing table, set of IP addresses,
socket listing, connection tracking table, firewall, and other network‑related resources.

- A `mount namespace <https://man7.org/linux/man-pages/man7/mount_namespaces.7.html>`_
has an independent list of mount points seen by the processes in the namespace. This means
that you can mount and unmount filesystems in a mount namespace without affecting the host filesystem.

- An `interprocess communication (IPC) namespace <https://man7.org/linux/man-pages/man7/ipc_namespaces.7.html>`_
has its own IPC resources.

- A `UNIX Time‑Sharing (UTS) namespace <https://man7.org/linux/man-pages/man7/uts_namespaces.7.html>`_
allows a single system to appear to have different host and domain names to different processes.




`What Are Namespaces and cgroups, and How Do They Work? <https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work>`_
