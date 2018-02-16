---
title: "Backporting Qt App from Python3 to Python2 (SIP API Versions)"
slug: backporting-qt-app-from-python3-to-python2-sip-api-versions
date: 2014-04-23T19:21:19+01:00
tags: ["PyQt4", "Python3", "Python2", "backporting", "API"]
categories: ["Blog"]
---

The issue came up with trying to integrate parts of an existing Python3/Qt app
(mikidown) into a Python2/Qt app.  After minor standard issues, like
exceptions and print functions, many strange errors with respect to ``QString``
and ``QVariant`` handling showed up. 

Searching for solution considering the different unicode string handling
between Py2 & Py3 did not give any answers: the solution lay in the different
``PyQt4`` API versions for the different Python versions. 

An introduction to the [incompatible PySide API][1] gave insight in how the
different versions are handled and how the used API version can be switched
for a given python environment.

The problems related to ``QString`` & ``QVariant`` could be solved by switching the 
API version for those two classes to the one which is standard in Python3. 

The following snippet needed to be inserted before importing any Qt related
module::

	import sip
	sip.setapi('QString', 2)
	sip.setapi('QVariant', 2)


``PySide`` only supports APIv2, I have to check ``PyQt5`` as well.

My application for this is for the development of Markdown/reStructured text
based wiki utilizing [mikidown][2] and [ReText][3]. Both target the `Python3` but 
with the trick above, large number of issues were resolved.

[1]: http://pyqt.sourceforge.net/Docs/PyQt4/incompatible_apis.html
[2]: https://github.com/rnons/mikidown
[3]: https://sourceforge.net/projects/retext/
