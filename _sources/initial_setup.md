# Initial Setup

The tech stack that I had to create and deploy this
book was:
* A Windows environment
* PyCharm Community Edition
* GitHub Desktop

Some mistakes I made in setup include:
* Installing and running Jupyter Book on Python 3.9
* Trying to install a g-zipped version of Python 3.7 to install the latest version (Python 3.7.12)
* Navigating to the gh-pages branch

Some challenges I faced in setup include:
* Deploying the sample book
* Activating a virtual environment in PyCharm after navigating between Git branches

Let's go through these mistakes in more detail:

## Installing and running Jupyter Book on Python 3.9

The latest version of Python today is Python 3.10, but
I routinely use Python 3.9 on my local machine.

I managed to install `jupyter-book` and successfully
ran the command:

```
jupyter-book create mynewbook/
```

So imagine the surprise I experienced, following
<a href='https://jupyterbook.org/start/create.html'>this
tutorial</a> when I ran

```
jupyter-book build book/
```

and got <a href='https://github.com/executablebooks/jupyter-book/issues/906'>
this problem</a>.

I wish I started by reading the warning
<a href='https://jupyterbook.org/start/your-first-book.html'>
here</a> and then navigating to 
<a href='https://jupyterbook.org/advanced/windows.html#working-on-windows'>
Working on Windows</a>

:::{admonition} Lesson 1:
:class: tip
Learn how to set up Jupyter Book based on your OS
environment.
:::

## Trying to install the latest version of Python 3.7

When I found that Python 3.7 was required, my natural
inclination was to go
<a href='https://www.python.org/downloads/'>here</a>
and zoomed in to Python 3.7.12:

![py1](./images/img1.png)

It came as a `.tgz` which I unzipped on Administrator
Windows `cmd` with the following code:

```
tar -xvzf C:\Users\CT\Downloads\Python-3.7.12.tgz -C C:\Users\CT\Downloads  
```

When I unzipped it, I felt stuck because I couldn't
activate it without a crazy amount of searching.

There had to be a better way.

So I decided to search for versions of Python 3.7
that had a `.exe` file for Windows
<a href='https://www.python.org/downloads/windows/'>
here</a>
settling on Python 3.7.9:

![py2](./images/img2.png)

I clicked the *Download Windows x86-64 executable
installer* because my machine is 64-bit. I installed
it with default installation instructions, and I was
able to create a virtual environment `venv` in my project
that ran on Python 3.7 with the following code:

```
# cd to \directory then run
virtualenv -p python3.7 venv
```

All that's left is activating `venv` and checking that
the correct version of Python is running with the code
below:

```
py --version
```

Finally, in Github Desktop after committing the files
created with

```
jupyter-book create mynewbook/
```

I added all `venv` files to `.gitignore`.