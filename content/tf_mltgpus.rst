.. _tf_mltgpus ::

Distributed training in TensorFlow
==================================

TensorFlow provides different methods to distribute training with minial coding.
``tf.distribute.Strategy`` is a TensorFlow API to distribute training across
multiple GPUs, multiple machines, or TPUs. Using this API, you can distribute
your existing models.

The main advanges of using ``tf.distribute.Strategy`` are:

- Easy to use and support multiple user segments,
  including researchers, machine learning engineers, etc.
- Provide good performance out of the box.
- Easy switching between strategies.

You can distribute training using ``tf.distribute.Strategy`` with a high-level
API like Keras ``Model.fit``, as we are familiar with, as well as custom training
loops (and, in general, any computation using TensorFlow).
You can use tf.distribute.Strategy with very few changes to your code, because
the underlying components of TensorFlow have been changed to become strategy-aware.
This includes variables, layers, models, optimizers, metrics, summaries, and checkpoints.

Types of strategies
___________________

``tf.distribute.Strategy`` covers several use cases along different axes.
Some of these combinations are currently supported. TensorFlow promises in the website
that others will be added in the future. Some of these axes are:

- **Synchronous vs asynchronous training**: These are two common ways of distributing
  training with data parallelism. In sync training, all workers train over different
  slices of input data in sync, and aggregating gradients at each step. In async training,
  all workers are independently training over the input data and updating variables asynchronously.
  Typically sync training is supported via all-reduce and async through parameter server architecture.

- **Hardware platform**: You may want to scale your training onto multiple GPUs on
  one machine, or multiple machines in a network (with 0 or more GPUs each), or on Cloud TPUs.

MirroredStrategy
++++++++++++++++

``tf.distribute.MirroredStrategy`` supports synchronous distributed training on
multiple GPUs on one machine. It creates one replica per GPU device. Each variable
in the model is mirrored across all the replicas. Together, these variables form
a single conceptual variable called MirroredVariable. These variables are kept
in sync with each other by applying identical updates.

Efficient all-reduce algorithms are used to communicate the variable updates across
the devices. All-reduce aggregates tensors across all the devices by adding them up,
and makes them available on each device. It’s a fused algorithm that is very efficient
and can reduce the overhead of synchronization significantly. There are many all-reduce
algorithms and implementations available, depending on the type of communication available
between devices. By default, it uses the NVIDIA Collective Communication Library (`NCCL <https://developer.nvidia.com/nccl>`_)
as the all-reduce implementation.
