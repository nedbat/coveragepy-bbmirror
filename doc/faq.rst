.. Licensed under the Apache License: http://www.apache.org/licenses/LICENSE-2.0
.. For details: https://bitbucket.org/ned/coveragepy/src/default/NOTICE.txt

.. _faq:

==================
FAQ and other help
==================


Frequently asked questions
--------------------------

**Q: How do I use coverage.py with nose?**

The best way to use nose and coverage.py together is to run nose under
coverage.py::

    coverage run $(which nosetests)

You can also use ``nosetests --with-coverage`` to use `nose's built-in
plugin`__, but it isn't recommended.

The nose plugin doesn't expose all the functionality and configurability of
coverage.py, and it uses different command-line options from those described in
coverage.py's documentation.  Additionally nose and its coverage plugin are
unmaintained at this point, so they aren't receiving any fixes or other
updates.

__ https://nose.readthedocs.io/en/latest/plugins/cover.html


**Q: How do I run nosetests under coverage.py with tox?**

Assuming you've installed tox in a virtualenv, you can do this in tox.ini::

    [testenv]
    commands = coverage run {envbindir}/nosetests

Coverage.py needs a path to the nosetests executable, but ``coverage run
$(which nosetests)`` doesn't work in tox.ini because tox doesn't handle the
shell command substitution. Tox's `string substitution`__ shown above does the
trick.

__ https://tox.readthedocs.io/en/latest/config.html#substitutions


**Q: I use nose to run my tests, and its coverage plugin doesn't let me create
HTML or XML reports. What should I do?**

First run your tests and collect coverage data with `nose`_ and its plugin.
This will write coverage data into a .coverage file.  Then run coverage.py from
the :ref:`command line <cmd>` to create the reports you need from that data.

.. _nose: https://nose.readthedocs.io/


**Q: Why do unexecutable lines show up as executed?**

Usually this is because you've updated your code and run coverage.py on it
again without erasing the old data.  Coverage.py records line numbers executed,
so the old data may have recorded a line number which has since moved, causing
coverage.py to claim a line has been executed which cannot be.

If you are using the ``-x`` command line action, it doesn't erase first by
default.  Switch to the ``coverage run`` command, or use the ``-e`` switch to
erase all data before starting the next run.


**Q: Why do the bodies of functions (or classes) show as executed, but the def
lines do not?**

This happens because coverage.py is started after the functions are defined.
The definition lines are executed without coverage measurement, then
coverage.py is started, then the function is called.  This means the body is
measured, but the definition of the function itself is not.

To fix this, start coverage.py earlier.  If you use the :ref:`command line
<cmd>` to run your program with coverage.py, then your entire program will be
monitored.  If you are using the :ref:`API <api>`, you need to call
coverage.start() before importing the modules that define your functions.


**Q: Coverage.py is much slower than I remember, what's going on?**

Make sure you are using the C trace function.  Coverage.py provides two
implementations of the trace function.  The C implementation runs much faster.
To see what you are running, use ``coverage debug sys``.  The output contains
details of the environment, including a line that says either
``tracer: CTracer`` or ``tracer: PyTracer``.  If it says ``PyTracer`` then you
are using the slow Python implementation.

Try re-installing coverage.py to see what happened and if you get the CTracer
as you should.


**Q: Isn't coverage testing the best thing ever?**

It's good, but `it isn't perfect`__.

__ https://nedbatchelder.com/blog/200710/flaws_in_coverage_measurement.html


..  Other resources
    ---------------

    There are a number of projects that help integrate coverage.py into other
    systems:

    - `trialcoverage`_ is a plug-in for Twisted trial.

    .. _trialcoverage: https://pypi.org/project/trialcoverage/

    - `pytest-coverage`_

    .. _pytest-coverage: https://pypi.org/project/pytest-coverage/

    - `django-coverage`_ for use with Django.

    .. _django-coverage: https://pypi.org/project/django-coverage/


**Q: Where can I get more help with coverage.py?**

You can discuss coverage.py or get help using it on the `Testing In Python`_
mailing list.

.. _Testing In Python: http://lists.idyll.org/listinfo/testing-in-python

Bug reports are gladly accepted at the `Bitbucket issue tracker`_.

.. _Bitbucket issue tracker: https://bitbucket.org/ned/coveragepy/issues

Announcements of new coverage.py releases are sent to the
`coveragepy-announce`_ mailing list.

.. _coveragepy-announce: http://groups.google.com/group/coveragepy-announce

`I can be reached`__ in a number of ways, I'm happy to answer questions about
using coverage.py.

__  https://nedbatchelder.com/site/aboutned.html


History
-------

Coverage.py was originally written by `Gareth Rees`_.
Since 2004, `Ned Batchelder`_ has extended and maintained it with the help of
`many others`_.  The :ref:`change history <changes>` has all the details.

.. _Gareth Rees:    http://garethrees.org/
.. _Ned Batchelder: https://nedbatchelder.com
.. _many others:    https://bitbucket.org/ned/coveragepy/src/tip/CONTRIBUTORS.txt
