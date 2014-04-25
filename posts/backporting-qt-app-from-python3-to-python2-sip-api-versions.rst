.. title: Backporting Qt App from Python3 to Python2 (SIP API Versions)
.. slug: backporting-qt-app-from-python3-to-python2-sip-api-versions
.. date: 2014/04/23 07:21:19
.. tags: PySide, Qt, Python3, Python2
.. link: 
.. description: 
.. type: text

The issue came up with trying to integrate parts of an existing Python3/Qt app
(mikidown) into a Python2/Qt app.  After minor standard issues, like
exceptions and print functions, many strange errors with respect to ``QString``
and ``QVariant`` handling showed up. 

Searching for solution considering the different unicode string handling
between Py2 & Py3 did not give any answers: the solution lay in the different
PySide API versions for the different Python versions. 

An introduction to the `incompatible PySide API`_ gave insight in how the
different versions are handled and how the used API version can be switched
for a given python environment.

The problems related to ``QString`` & ``QVariant`` could be solved by switching the 
API version for those two classes to the one which is standard in Python3. 

The following snippet needed to be inserted before importing any Qt related
module::

	import sip
	sip.setapi('QString', 2)
	sip.setapi('QVariant', 2)

My application for this is for the development of Markdown/reStructured text
based wiki utilizing `mikidown`_ and `ReText`_. Both target the `Python3` but 
with the trick above, large number of issues were resolved.

.. _incompatible PySide API: http://pyqt.sourceforge.net/Docs/PyQt4/incompatible_apis.html
.. _mikidown: https://github.com/rnons/mikidown
.. _ReText: https://sourceforge.net/projects/retext/
