.. Preface
.. ===============================================================================

前言
===============================================================================

.. contents:: **目录**
   :local:


.. About the author
.. ----------------

关于作者
----------------

`Nicolas P. Rougier`_ is a full-time research scientist at Inria_ which is the
French national institute for research in computer science and control. This is
a public scientific and technological establishment (EPST) under the double
supervision of the Research & Education Ministry, and the Ministry of Economy
Finance and Industry. Nicolas P. Rougier is working within the Mnemosyne_
project which lies at the frontier between integrative and computational
neuroscience in association with the `Institute of Neurodegenerative
Diseases`_, the Bordeaux laboratory for research in computer science
(LaBRI_), the `University of Bordeaux`_ and the national center for scientific
research (CNRS_).

He has been using Python for more than 15 years and numpy for more than 10
years for modeling in neuroscience, machine learning and for advanced
visualization (OpenGL). Nicolas P. Rougier is the author of several online
resources and tutorials (Matplotlib, numpy, OpenGL) and he's teaching Python,
numpy and scientific visualization at the University of Bordeaux and in various
conferences and schools worldwide (SciPy, EuroScipy, etc). He's also the author
of the popular article `Ten Simple Rules for Better Figures`_ and a popular
`matplotlib tutorial
<http://www.labri.fr/perso/nrougier/teaching/matplotlib/matplotlib.html>`_.


.. About this book
.. ---------------

关于本书
---------------
这本书是用 |ReST|_ 格式撰写，通过 docutils_ 中的 `rst2html.py` 命令编译生成。

.. This book has been written in |ReST|_ format and generated using the
.. `rst2html.py` command line available from the docutils_ python package.

如果你想重新生成本书的html版本，请在本书根目录中运行命令：

.. If you want to rebuild the html output, from the top directory, type:

.. code-block::

   $ rst2html.py --link-stylesheet --cloak-email-addresses \
                 --toc-top-backlinks --stylesheet=book.css \
                 --stylesheet-dirs=. book.rst book.html

源代码在https://github.com/rougier/from-python-to-numpy.

.. The sources are available from https://github.com/rougier/from-python-to-numpy.
                   
.. |ReST| replace:: restructured text
.. _ReST: http://docutils.sourceforge.net/rst.html
.. _docutils: http://docutils.sourceforge.net/


.. Prerequisites
.. +++++++++++++

预备知识
+++++++++++++

这不是一本Python入门书，你需要熟悉Python，并了解Numpy。
如果你没有这些知识，可先学习 bibliography_ 中列举的内容。

.. This is not a Python beginner guide and you should have an intermediate level in
.. Python and ideally a beginner level in numpy. If this is not the case, have
.. a look at the bibliography_ for a curated list of resources.


.. Conventions
.. +++++++++++

排版约定
+++++++++++

我们将使用一些命名约定：如果没有特别说明，那么默认 numpy, scipy 和 matplotlib 三个库的导入方式如下:

.. We will use usual naming conventions. If not stated explicitly, each script
.. should import numpy, scipy and matplotlib as:

.. code-block:: python
   
   import numpy as np
   import scipy as sp
   import matplotlib.pyplot as plt

我们使用的是当前最新版的库，版本如下：

.. We'll use up-to-date versions (at the date of writing, i.e. January, 2017) of the
.. different packages:

=========== =========
Packages    Version
=========== =========
Python      3.6.0
----------- ---------
Numpy       1.12.0
----------- ---------
Scipy       0.18.1
----------- ---------
Matplotlib  2.0.0
=========== =========

.. How to contribute
.. +++++++++++++++++

如何为本书做贡献
+++++++++++++++++

如果你想参与本书创作，你可以：

.. If you want to contribute to this book, you can:


* 审核章节（请先联系我）
* 报告问题 (https://github.com/rougier/from-python-to-numpy/issues)
* 语言修正 (https://github.com/rougier/from-python-to-numpy/issues)
* 为本书设计一个更好的html模版
* 在Github上点赞 (https://github.com/rougier/from-python-to-numpy)

.. * Review chapters (please contact me)
.. * Report issues (https://github.com/rougier/from-python-to-numpy/issues)
.. * Suggest improvements (https://github.com/rougier/from-python-to-numpy/pulls)
.. * Correct English (https://github.com/rougier/from-python-to-numpy/issues)
.. * Design a better and more responsive html template for the book.
.. * Star the project (https://github.com/rougier/from-python-to-numpy)

.. Publishing

出版
++++++++++

如果你是一个编辑，对出版本书有兴趣，你可以 `联系我
<mailto:Nicolas.Rougier@inria.fr>`_。 
但有几个要求：同意这个在线版本以及后续版本的存在和开放获取，(即`这个地址
<http://www.labri.fr/perso/nrougier/from-python-to-numpy>`_)。
你需要知道用 `restructured text <http://docutils.sourceforge.net/rst.html>`_ 排版而不是 Word。
能提供高质量的支持服务，更重要的是，有一个超赞的latex模版。

.. If you're an editor interested in publishing this book, you can `contact me
.. <mailto:Nicolas.Rougier@inria.fr>`_ if you agree to have this version and all
.. subsequent versions open access (i.e. online at `this address
.. <http://www.labri.fr/perso/nrougier/from-python-to-numpy>`_), you know how to
.. deal with `restructured text <http://docutils.sourceforge.net/rst.html>`_ (Word
.. is not an option), you provide a real added-value as well as supporting
.. services, and more importantly, you have a truly amazing latex book template
.. (and be warned that I'm a bit picky about typography & design: `Edward Tufte
.. <https://www.edwardtufte.com/tufte/>`_ is my hero). Still here?


.. License

许可
--------

.. **Book**

**书**

本书采用如下许可证 `Creative Commons Attribution-Non Commercial-Share
Alike 4.0 International License <https://creativecommons.org/licenses/by-nc-sa/4.0/>`_。
你可以：

.. This work is licensed under a `Creative Commons Attribution-Non Commercial-Share
.. Alike 4.0 International License <https://creativecommons.org/licenses/by-nc-sa/4.0/>`_. You are free to:

* **Share** — copy and redistribute the material in any medium or format
* **Adapt** — remix, transform, and build upon the material

The licensor cannot revoke these freedoms as long as you follow the license terms.

.. **Code**

**代码**

The code is licensed under the `OSI-approved BSD 2-Clause License
<LICENSE-code.txt>`_.


.. --- Links ------------------------------------------------------------------
.. _Nicolas P. Rougier:     http://www.labri.fr/perso/nrougier/
.. _Inria:                  http://www.inria.fr/en
.. _Mnemosyne:              http://www.inria.fr/en/teams/mnemosyne
.. _LaBRI:                  https://www.labri.fr/
.. _CNRS:                   http://www.cnrs.fr/index.php
.. _University of Bordeaux: http://www.u-bordeaux.com/
.. _Institute of Neurodegenerative Diseases:
      http://www.imn-bordeaux.org/en/
.. _Ten Simple Rules for Better Figures:
      http://dx.doi.org/10.1371/journal.pcbi.1003833
.. ----------------------------------------------------------------------------

