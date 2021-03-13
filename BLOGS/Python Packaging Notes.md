# Python Packaging Notes
Inspired From : https://packaging.python.org/
Python Packaging User Guide, a collection of tutorials and references to help you distribute and install Python packages with modern tools.

**But first ~~ coffee ~~ 

**Terminology:**
But what is a package you may ask. Lets learn about packaging soon as we go through some basic terms:

**Modules** in Python are simply Python files with a .py extension. The name of the module will be the name of the file. A Python module can have a set of functions, classes or variables defined and implemented

**Packages** are namespaces which contain multiple packages and modules themselves. They are simply directories, but with a twist. Each package in Python is a directory which MUST contain a special file called *__init_*.py. This file can be empty, and it indicates that the directory it contains is a Python package, so it can be imported the same way a module can be imported.

Python **library** is a collection of functions and methods that allows you to perform many actions without writing your code. Unlike C or C++, the term library does not have any specific contextual meaning in Python. Basically In Python, a library is used loosely to describe a collection of the core modules.

In summary:
a **Python module** in python is a .py file that defines one or more function/classes which you intend to reuse in different codes of your program.
a **Python package** refers to a directory of Python module(s). This feature comes in handy for organizing modules of one type at one place.
a **Python library** is used loosely to describe a collection of the core modules for a specific domain.

Python is a very versatile language. It is strange but helpful to think about packaging before writing actual code. This is because Python provides multiple options for packaging and you should know beforehand which of these serve you best in deploying your project.

**Thinking about deployment** is essential in terms of: 
1. end users:  tech v/s non-tech people | developers or operations or students . . .
2. Machine to be used on: data center/ sever / desktops/ mobile phones/ ..,
3. for individual usage v/s collaborative project.


**1. Packaging Python Libraries and tools**: libraries and tools used by technical audience in a development setting

- ***common tools:***  PyPI, setup.py, and wheel

- ***1.Modules:*** a python file that  relies only on standard library.
easy to share over email/forums/ Github gist. **BUT!**  this pattern won’t scale for projects that consist of multiple files, need additional libraries, or need a specific version of Python, hence the options below.

- ***2.Souce Distributions (sdist):*** sdists are compressed archives (.tar.gz files) containing one or more packages or modules.
A. If your code is **pure-Python**, and you only depend on other Python packages, you can read [ python docs for this](https://docs.python.org/3/distutils/sourcedist.html).
B. If you rely on any non-Python code, or non-Python packages, see next topic(Binary distributions)

- ***3.Binary distributions:***  To integrate with libraries written in C++, C, Rust, etc. 
Uses [wheel](https://pythonwheels.com/) to ship libraries having compiled pieces.
Python’s package installer, pip, always prefers wheels because installation is always faster, so even pure-Python packages work better with wheels. 

	*Binary distributions are best when they come with source distributions to match. Even if you don’t upload wheels of your code for every operating system, by uploading the sdist, you’re enabling users of other platforms to still build it for themselves.*


Till now we can generalize that these built-in approaches to be meant for only target environments which have Python, and an audience who knows how to install Python packages. With the variety of operating systems, configurations, and people out there, this assumption is only safe when targeting a developer audience.

>Python's native packaging is meant for ***distributing*** reusable code (aka libraries)  between developes. But these libraries are building blocks and not actual application. To learn more about distributing applications, see the section below. 

**2.Packaging Python Applications:** Again Python being a versatile language offers multiple options for packaging applications. Your choice for choosing specific tools depend on a number of factors. Consideration can be based on following:

- **A. Depending on framework:** For some technical domains there are full frameworks available for deployment and packaging purpose.

	**1. Service apps**: you can develop a service application using a PaaS like Google App, Heroku which have their own set of tools/workflows to be followed.
		- [Google App](https://cloud.google.com/appengine/docs/python/): enables you to use Google’s proven serving technology to build your web, mobile and IoT applications quickly and with minimal operational overhead. Provides code-centric developer workflow, scale quickly and efficiently to handle increasing demand.
		- [Heroku](https://devcenter.heroku.com/articles/getting-started-with-python) : Heroku is a service that enables companies to develop and deploy apps.
		- [PythonAnywhere](https://www.pythonanywhere.com/): make a complicated process very simple, focus on creating exciting applications for your users. No need to manage a web server or maintain a Linux machine. No need to install security patches.
		- [RedHat OpenShift](https://www.openshift.com/try): is a leading hybrid cloud, enterprise Kubernetes application platform.
		these platforms takes care of packaging and deployment, as long as you follow their patterns. Most software does not fit one of these templates, hence the existence of all the other options below. If you’re developing software that will be deployed to machines you own, users’ personal computers, or any other arrangement, read on.
		
	**2. Mobile/Web**: These days you can write a mobile app or web application frontend in Python. While the language may be familiar, the packaging and deployment practices are brand new.
		-  [Kivy](https://kivy.org/#home): Open source Python library for rapid development of applications that make use of innovative user interfaces, such as multi-touch apps. 
		- [Beeware](https://beeware.org/): Write your apps in Python and release them on iOS, Android, Windows, MacOS, Linux, Web, and tvOS using rich, native user interfaces. Multiple apps, one codebase, with a fully native user experience on every platform.
		-[Brython](https://brython.info/): A Python 3 implementation for client-side web programming.
		-[Flexx](https://flexx.readthedocs.io/en/latest/index.html): You can use Flexx to create (cross platform) desktop applications, web applications, and export an app to a standalone HTML document. It also works in the Jupyter notebook.

- ** B. Using pre-installed Python:** since most of the devices have python on them, you can use following options to directly run you package (like an excecutable zip file) on these devices. You can make such files using these:

	- [PEX](https://github.com/pantsbuild/pex#pex) (Python EXecutable)
	- [zipapp](https://docs.python.org/3/library/zipapp.html) (does not help manage dependencies, requires Python 3.5+)
	- [shiv](https://github.com/linkedin/shiv#shiv) (requires Python 3)
	
- **C. Depending on a separate software distribution ecosystem:** earlier window, and MacOS lacked built-in package managers, even now the app-stores are centered more towards users and less towards developers. Solution? a dedicated package management tool. 
	Examples:
	- [Anaconda](https://www.anaconda.com/)
	- [Homebrew](https://brew.sh/)
	- [Enthought](https://www.enthought.com/)
	- [ActiveState](https://www.activestate.com/products/python/)
	- [WinPython](https://winpython.github.io/#overview)

- **D. Bringing your own executable:** Every operating system natively supports one or more formats of program they can natively execute.
There are many techniques and technologies which turn your Python program into one of these formats, most of which involve embedding the Python interpreter and any other dependencies into a single executable file.
*This approach, called **freezing**, offers wide compatiblity and seamless user experience, though often requires multiple technologies, and a good amount of effort.*
Examples:
	- [pyInstaller](https://www.pyinstaller.org/) - Cross-platform
	PyInstaller freezes (packages) Python applications into stand-alone executables, under Windows, GNU/Linux, Mac OS X, FreeBSD, Solaris and AIX.
	
	- [constructor](https://github.com/conda/constructor) - For command-line installers
	Constructor is a tool which allows constructing an installer for a collection of conda packages
	
	- [py2exe](https://www.py2exe.org/) - Windows only
	py2exe is a Python Distutils extension which converts Python scripts into executable Windows programs, able to run without requiring a Python installation.
	
	- [py2app](https://py2app.readthedocs.io/en/latest/) - Mac only
	py2app is a Python setuptools command which will allow you to make standalone application bundles and plugins from Python scripts. 
	
	- [bbFreeze](https://pypi.org/project/bbfreeze/) - Windows, Linux, Python 2 only
	bbfreeze creates stand-alone executables from python scripts.
	
	- [osnap](https://github.com/jamesabel/osnap) - Windows and Mac
	OSNAP is a way to deliver self-contained Python applications to end users for Windows and OSX/MacOS.
	
	- [pynsist](https://pypi.org/project/pynsist/) - Windows only
	Pynsist is a tool to build Windows installers for your Python applications. The installers bundle Python itself, so you can distribute your application to people who don’t have Python installed.
	
- **E. On containers** While using OS containers, we can deploy package along with OS image. These techniques are mostly Python agnostic, because they package whole OS filesystems, not just Python or Python packages.
	Examples:
	- [Docker](https://docs.docker.com/): Docker can package up applications along with their necessary operating system dependencies for easier deployment across environments. In the long run it has the potential to be the abstraction layer that easily manages containers running on top of any type of server, regardless of whether that server is on Amazon Web Services, Google Compute Engine, Linode, Rackspace or elsewhere.
	
	- [AppImage](https://appimage.org/): Distribute your desktop Linux application in the AppImage format and win users running all common Linux distributions. Package once and run everywhere. Reach users on all major desktop distributions. 
	
	- [FlatPak](https://flatpak.org/): Create one app and distribute it to the entire Linux desktop market.
	
	- [SnapCraft](https://snapcraft.io/): Publish your app for Linux users — for desktop, cloud, and Internet of Things. ***Snaps*** are app packages for desktop, cloud and IoT that are easy to install, secure, cross-platform and dependency-free.
		
	    **snap** is both the command line interface and the application package format
	    **snapd** is the background service that manages and maintains your snaps
	    **snapcraft** is the command and the framework used to build your own snaps
	    **Snap Store** provides a place to upload your snaps, and for users to browse and install
	
- **F. Using Classic Virtualization**; Of creating applications to run OS images. Technologies are Python agnostic, and include:
    - [Vagrant](https://www.vagrantup.com/)
    - [VHD](https://en.wikipedia.org/wiki/VHD_(file_format)) , [AMI](https://en.wikipedia.org/wiki/Amazon_Machine_Image) , and other formats
    - [OpenStack](https://www.redhat.com/en/topics/openstack) - A cloud management system in Python, with extensive VM support.
    
- **G. Using your own Hardware**: Embed your code on an Adafruit, MicroPython, or more-powerful hardware running Python, then ship it to the datacenter or your users’ homes.
Example:
	- [Adafruit](https://github.com/adafruit/circuitpython#adafruit-circuitpython) 
	- [MicroPython](https://micropython.org/) 

-**Using OS package managers**: for most of the Linux OSes we have a mature package manager. We can rely on formats like *deb* in case of Ubuntu or Debian, and *RPM* for RedHat, Fedora, and use those package managers for install as well as deployment.
Can also use [FPM](https://fpm.readthedocs.io/en/latest/source/virtualenv.html) to generate deb and RPMs from same source.

**More tools**
1. Usage of [virtualenvs](http://python-guide.readthedocs.io/en/latest/dev/virtualenvs/)  has been extensive and is now futher enhanced by wrapping them in a self-contained way. see: [dh-virtualenv](dh-virtualenv.readthedocs.io/en/1.0/tutorial.html) and [osnap](https://github.com/jamesabel/osnap) READ MORE!

2. Security
3. Static v/s Dynamic linking

**Wrap Up**: Packaging in Python has a bit of a reputation for being a bumpy ride. This impression is mostly a byproduct of Python’s versatility. Once you understand the natural boundaries between each packaging solution, you begin to realize that the varied landscape is a small price Python programmers pay for using one of the most balanced, flexible language available.


**Some important libraries:**
Each library in Python contains a huge number of useful modules that you can import for your every day programming. 
There are 20 Python Libraries that you cannot live without:

- Requests. HTTP library.
- Scrapy. For web scraping.
- wxPython.A GUI toolkit.
- Pillow. Imaging library.
- SQLAlchemy. A database library.
- Beautiful Soup. XML and HTML parsing library.
- Twisted. For networking applications development.
- Numpy. Provides math functions.
- SciPy. Algorithm and mathematical tools.
- matplotlib.a numerical plotting library.
- Pygame. 2d game development.
- Pyglet. A 3d animation and game creation engine.
- pyQT. For GUI development.
- pywin32.for interacting with Windows.
- pyGtk. Python GUI library.
- nltk. Manipulation of strings.
- Nose. A testing framework for Python.
- IPython. Shell for Python.
- Sympy. Algebraic evaluation, complex numbers, differentiation.
- Scrapy. A packet sniffer and analyzer.

There are 15 Python libraries for data science, from information extraction to deep learning models. Python uses its rich libraries, easy of use and efficient nature to beat over R and become the data science solutions.

- Beautiful Soup. Extract info from HTML and XML.
- Numpy. Scientific computing.
- Scrapy. Extract data and web crawler.
- SciPy. Signal processing, optimization and statistics.
- Pandas. Data manipulation and analysis.
- Scikit-learn. machine learning and data mining.
- Tensorflow. Machine learning and deep learning.
- Karas. Neural networks API. Supports deep learning.
- PyTorch. Neural network modeling libary with GUI.
- NLTK. Language processing.
- SpaCy. Large scale extracting and analysing of textual information. Support deep learning.
- matplotlib. For data visualization.
- Seaborn. Also for data visualization. Also support for pandas and Numpy.
- Bokeh. Support large-scale interactivity and visualizations of real-time data sets.
- Plotly. For making publication-quality plots and graphs. Widely used in finance and geospatial industries.


