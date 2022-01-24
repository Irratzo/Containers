Creating More Complex Container Images
======================================

In order to create and use your own containers, you may need more
information than our previous example. You may want to use files from
outside the container, copy those files into the container, and just
generally learn a little bit about software installation. This episode
will cover these. Note that the examples will get gradually more and
more complex - most day-to-day use of containers can be accomplished
using the first 1-2 sections on this page.

Using scripts and files from outside the container
--------------------------------------------------

Let's create a file and folder called it ``foo/dummy.py`` in the root folder.

Please copy the ``Dockerfile`` and place it in the ``foo`` directory.
Let’s say we wanted to try running the script using our recently created ``alpine-python`` container.

.. callout :: Running containers

   What command would we use to run python from the ``alpine-python``
   container?

If we try running the container and Python script, what happens?

.. code-block :: bash
  $docker run alice/alpine-python python3 sum.py

Output
..code-block :: bash
  python3: can’t open file ‘dummy.py’: [Errno 2] No such file or directory

.. callout :: No such file or directory

   What does the error message mean? Why might the Python inside the
   container not be able to find or open our script?

The problem here is that the container and its file system is separate
from our host computer’s file system. When the container runs, it can’t
see anything outside itself, including any of the files on our computer.
In order to use Python (inside the container) and our script (outside
the container, on our computer), we need to create a link between the
directory on our computer and the container.

This link is called a “mount” and is what happens automatically when a
USB drive or other external hard drive gets connected to a computer -
you can see the contents appear as if they were on your computer.

We can create a mount between our computer and the running container by
using an additional option to ``docker run``. We’ll also use the
variable ``$PWD`` which will substitute in our current working
directory. The option will look like this

.. code-block :: bash

  ``-v $PWD:/temp``

What this means is – link my current directory with the container, and
inside the container, name the directory ``/temp``

Let’s try running the command now:

.. code-block :: bash

  $ docker run -v $PWD:/temp alice/alpine-python python3 dummy.py
