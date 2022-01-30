.. _tf_intro ::

TensorFlow on a single GPU
==========================

TensorFlow is a well-known library developed primarily in Google which has been
proven to be one of the most robust, reilable, and fast libraries for deep learning
among developers. I think most of us have had some form of exposure to TensorFlow
at some point in our deep learning/machin learning journery.

In this section we focus on using a single GPU for training our model. It is rather
easy to transfer/port traning of the model to the GPU with minimal coding.

TensorFlow supports running computations on a variety of types of devices, including
CPU and GPU. They are represented with string identifiers for example:

  - ``/device:CPU:0``: The CPU of your machine.
  - ``/GPU:0``: Short-hand notation for the first GPU of your machine that is
    visible to TensorFlow.
  - ``/job:localhost/replica:0/task:0/device:GPU:1``: Fully qualified name of
    the second GPU of your machine that is visible to TensorFlow.

If a TensorFlow operation has both CPU and GPU implementations, by default,
the GPU device is prioritized when the operation is assigned. For example, ``tf.matmul``
has both CPU and GPU kernels and on a system with devices ``CPU:0`` and ``GPU:0``,
the ``GPU:0`` device is selected to run ``tf.matmul`` unless you explicitly request
to run it on another device.

If a TensorFlow operation has no corresponding GPU implementation, then the operation
falls back to the CPU device. For example, since ``tf.cast`` only has a CPU kernel,
on a system with devices ``CPU:0`` and ``GPU:0``, the ``CPU:0`` device is selected
to run ``tf.cast``, even if requested to run on the ``GPU:0`` device.

Let's check if we have TensorFlow module loaded and if the physical GPU device is
available.

.. code-block :: bash

  import tensorflow as tf
  print("Num of GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
  print("TensorFlow version: ", tf.__version__)

.. code-block :: bash

  Num of GPUs Available:  6
  TensorFlow version:  2.5.0

We can see the list of all of available devices:

.. code-block :: bash

  tf.config.list_physical_devices()

.. code-block :: bash

  [PhysicalDevice(name='/physical_device:CPU:0', device_type='CPU'),
  PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU'),
  PhysicalDevice(name='/physical_device:GPU:1', device_type='GPU'),
  PhysicalDevice(name='/physical_device:GPU:2', device_type='GPU'),
  PhysicalDevice(name='/physical_device:GPU:3', device_type='GPU'),
  PhysicalDevice(name='/physical_device:GPU:4', device_type='GPU'),
  PhysicalDevice(name='/physical_device:GPU:5', device_type='GPU')]

If you have GPUs, then you should see the GPU device in the above list.
We can also check specifically for the GPU or CPU devices.

.. code-block :: python

  tf.config.list_physical_devices('GPU')
  tf.config.list_physical_devices('CPU')

Let's define a random Tensor, and check where it is placed.

.. code-block :: python

  x = tf.random.uniform([3, 3])
  x.device

.. code-block :: bash

  '/job:localhost/replica:0/task:0/device:GPU:0'
