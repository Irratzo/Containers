.. _intro-container:

Introduction to Containers
==========================

.. image:: img/containers.jpeg
   :width: 45%

.. image:: img/containers_amazon.jpeg
   :width: 35%

When software is deployed, a set of libraries and configuration files is used in a runtime environment.
Typically, we have several applications running in the runtime environment.
Therefore, if a system update changes a lib to fix an issue, it might break other apps that use the same library.
We all have experienced this at some point.

As a developer, you need to control the version of libraries within the runtime environment.
Two technologies that can help you achieve this goal are Containers and Virtual Machines (VMs).
Managing the environment of the apps becomes possible with the help of "virtualization."
The system resources, e.g., RAM, CPU, storage, networking, can be "virtually" delivered as multiple resources in the virtualization process.

Containers are executable units of software that encapsulate everything to run. In principle, one can run containers anywhere.
Containers use the operating system (OS) level virtualization, isolating processes from
the rest of the OS environment and controlling the allocation of (hardware) resources. The isolation is enabled via kernel namespaces
and cgroups (as we will discuss them in detail at :doc:`namespc-cgroup` section), which have been in Linux for a long time.

The key differentiator between containers and VMs is that VMs virtualize an entire machine down to the hardware layers and containers only virtualize software layers above the operating system level.

.. image:: img/conts_vms.jpeg

Cons and Pros of Containers
___________________________
Pros
++++
- Containers are lightweight software packages that contain all the dependencies.
- Because of their lightweight, it is easy and very fast to iteratively modify them.
Cons
++++
- Since containers share the same underlying hardware system, it is possible that an exploit in one container could break out of the container and affect the shared hardware.

Cons and Pros of VMs
____________________
Pros
++++
- VMs are immune to any exploits or interference from other VMs on a shared host due run in isolation as because of a fully standalone system.
- Since VMs are full-flegded OS, they are more dynamic and can be interactively developed. Once the basic hardware definition is specified for a VM, the VM can then be treated as a bare bones computer.

Cons
++++
- It is time consuming to build and regenerate VMs, because they encompass a full stack system. Any modifications to a VM snapshot can take significant time to regenerate and validate they behave as expected.
- VMs can take up a lot of storage space. They can quickly grow to several Gigabytes in size. This can lead to disk space shortage issues on the VMs host machine.
