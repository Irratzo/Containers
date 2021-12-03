.. _intro-container:

Introduction to Containers
==========================

Containers are executable units of software that encapsulate everything to run. In principle, one can run containers anywhere. Containers use 
the operating system (OS) level virtualization, isolating processes from 
the rest of the OS environment and controlling the allocation of (hardware) resources.

The isolation is enabled via kernel namespaces and cgroups (as we will discuss them in detail at :doc: `namespc-cgroup` section), which have been in Linux for a long time.

.. image:: img/containers.jpeg
   :width: 45%
   
.. image:: img/containers_amazon.jpeg
   :width: 35%
   


