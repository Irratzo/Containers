.. _hvd_intro ::

Intoduction to Horovod
======================

.. image :: https://horovod.readthedocs.io/en/stable/_static/logo.png
  :alt: Horovod

Why Horovod
___________

Horovod was developed at Uber with the primary motivation of making it easy to
take a single-GPU training script and successfully scale it to train across many
GPUs in parallel. This has two aspects:

- How much modification does one have to make to a program to make it distributed,
  and how easy is it to run it?
- How much faster would it run in distributed mode?

What researchers at Uber discovered was that the MPI model to be much more straightforward
and require far less code changes than previous solutions such as Distributed TensorFlow with
parameter servers. Once a training script has been written for scale with Horovod, it can run
on a single-GPU, multiple-GPUs, or even multiple hosts without any further code changes.

In addition to being easy to use, Horovod is fast. Below is a chart representing the benchmark
that was done on 128 servers with 4 Pascal GPUs each connected by RoCE-capable 25 Gbit/s network:

.. image :: https://user-images.githubusercontent.com/16640218/38965607-bf5c46ca-4332-11e8-895a-b9c137e86013.png
  :alt: scaling

Horovod achieves 90% scaling efficiency for both Inception V3 and ResNet-101, and
68% scaling efficiency for VGG-16. While installing MPI and NCCL itself may seem like an extra hassle,
it only needs to be done once by the team dealing with infrastructure, while everyone else in the company
who builds the models can enjoy the simplicity of training them at scale. Plus, in modern clusters where
GPUs are available, MPI and NCCL are readily installed. Installation of Horovod is not as difficult.

Main concept
____________

Horovod core principles are based on the MPI concepts *size*, *rank*, *local rank*,
*allreduce*, *allgather*, *broadcast*, and *alltoall*. These are best explained by example.
Say we launched a training script on 4 servers, each having 4 GPUs. If we launched one copy of the script per GPU:

- **Size** would be the number of processes, in this case, 16.
- **Rank** would be the unique process ID from 0 to 15 (size - 1).
- **Local rank** would be the unique process ID within the server from 0 to 3.
- **Allreduce** is an operation that aggregates data among multiple processes and
  distributes results back to them. Allreduce is used to average dense tensors.

  .. image :: http://mpitutorial.com/tutorials/mpi-reduce-and-allreduce/mpi_allreduce_1.png
  :alt: Allreduce
    :alt: allreduce

- **Allgather** is an operation that gathers data from all processes on every process.
  Allgather is used to collect values of sparse tensors.

  .. image :: http://mpitutorial.com/tutorials/mpi-scatter-gather-and-allgather/allgather.png
    :alt: allgather

- **Broadcast** is an operation that broadcasts data from one process, identified by
  root rank, onto every other process.

  .. image :: http://mpitutorial.com/tutorials/mpi-broadcast-and-collective-communication/broadcast_pattern.png
    :alt: broadcast

- **Alltoall** is an operation to exchange data between all processes.
  Alltoall may be useful to implement neural networks with advanced architectures that span multiple devices.
