# Tooling

## .NET Projects

You'll need to install the following tools in order to compile the different projects

- Microsoft .NET Framework 4.6.2 Developer Pack and Language Packs from [Microsoft Download Center](https://www.microsoft.com/en-us/download/confirmation.aspx?id=53321)
- Microsoft Visual Studio 2015 from https://www.visualstudio.com/downloads/


## Dashboard Frontend

The Jobbr Dashboard frontend is leveraging the following tools:

- [Aurelia](https://aurelia.io/) as web framework
- [Node.js](https://nodejs.org/) JavaScript runtime to build everything
- [NPM](https://www.npmjs.com/) as package manager
- [Aurelia CLI](https://aurelia.io/docs/cli/basics/) as Aurelia build and run tool
- [NVM](https://github.com/nvm-sh/nvm) or rather [NVM-Windows](https://github.com/coreybutler/nvm-windows) is optional, but a great way to switch between node versions

### Pre-Requisites

Install Node.js v10, as is currently required by the Jobbr Dashboard.

- Install nvm-windows with [the setup](https://github.com/coreybutler/nvm/releases) or with `choco install nvm`
- *You may need to restart your terminal*
- Run `nvm install 10`
- Run `nvm use 10.24.1`
- Clone the [jobbr-dashboard](https://github.com/jobbrIO/jobbr-dashboard) repository

### Building

- Navigate to `source/Jobbr.Dashboard.Frontend/jobbr-dashboard/` in your terminal
- Run `npm install`
- Run `npm install -g aurelia-cli`
- Run `au build --env prod`
  - Alternatively you can also choose `dev` or `stage` instead of `prod`

### Running

If you just want to confirm that the Dashboard loads correctly you can use:

- Run `au run`
- Open your browser at [http://localhost:1337](http://localhost:1337)

If you want a fully functioning Jobbr Dashboard:

- Run `au build --env prod`
- Zip up the whole content of the generated `source/Jobbr.Dashboard.Frontend/jobbr-dashboard/dist` directory excluding any subdirectories (e.g. img)
- Copy the zip file to the repository root
- Rename the zip file to `dashboard-app.zip`
- Build the `Sample.Jobbr.Server` project
- Start the `Sample.Jobbr.Server` project
- Open your browser at [https://localhost:1338](https://localhost:1338)


## Documentation

The documentation is build by the readthedocs.org-Service. If you need to develop the documentation locally, install the following tools

- [Python 3](https://docs.python-guide.org/starting/install3/win/)
- [Sphinx](https://www.sphinx-doc.org/en/master/usage/installation.html)
- [MyST-Parser](https://github.com/executablebooks/MyST-Parser)
- [Read the Docs Sphinx Theme](https://github.com/readthedocs/sphinx_rtd_theme)

Build the documentation with `make clean & make html` and open the index in your browser.

**Autobuild**
We added a autobuild option the the make.bat but you need to install shpinx-autostart in order to work. Instructions: [GaretJax/sphinx-autobuild](https://github.com/GaretJax/sphinx-autobuild)

When installed, you can just issue `make livehtml` and point your browser to [http://localhost:8000](http://localhost:8000)

### Basic Install Guide for Windows

**Note**: This guide uses [Chocolatey](https://chocolatey.org/), if you don't want to [install Chocolatey](https://chocolatey.org/install) see the extended guide below

1. Install Sphinx

    ```
    choco install sphinx
    ```

2. Restart your Terminal

    The Sphinx installation above also installed Python 3, which should be in your PATH once your restart the command line

3. Install MyST-Parser extension

    ```
    pip install myst-parser
    ```

4. Install the ReadTheDocs Sphinx Theme

    ```
    pip install sphinx_rtd_theme
    ```

### Extended Install Guide for Windows

1. Download and install [Python >= 3.5.2 from the official download location](https://www.python.org/downloads/). 

    > **Add Python to PATH**: Make sure you check the Option to add Python to the PATH or you'll nee to patch the `PATH` Environment variable manually.

    Check your installation by executing `py` in the console. You should see something like this and you'll be able to exit by pressing `Ctrl-Z` and hit enter. 

    ```
    C:\users\michael>py
    Python 3.10.8 (tags/v3.10.8:aaaf517, Oct 11 2022, 16:50:30) [MSC v.1933 64 bit (AMD64)] on win32
    Type "help", "copyright", "credits" or "license" for more information.
    ```
    > **New Console**: Keep in mind that environment variables (such as the `PATH`) will not change unless you start a new process with a console. That also applies to editors that host a console on their own.

2. Check PIP (The Python Package manager)
    
    The package manager should be installed, just check with the command `pip --version`:

    ```
    C:\Users\michael>pip --version
    pip 22.3 from C:\Python310\lib\site-packages\pip (python 3.10)
    ```
    If there is no such command
    * Make sure you have restarted you console
    * Install pip by the command `py get-pip.py`

3. Install Sphinx

    Sphinx is the document rendering engine that is also used to generate jobbr.readthedocs.io. The following command will install Sphinx and all other related packages in your Python installation.

    ```
    C:\Users\michael>pip install sphinx
    Collecting sphinx
    Downloading Sphinx-1.6.2-py2.py3-none-any.whl (1.9MB)
        100% |████████████████████████████████| 1.9MB 618kB/s
    Collecting snowballstemmer>=1.1 (from sphinx)

    ...

    Successfully installed Jinja2-2.9.6 MarkupSafe-1.0 Pygments-2.2.0 alabaster-0.7.10 babel-2.4.0 certifi-2017.4.17 
    chardet-3.0.4 colorama-0.3.9 docutils-0.13.1 idna-2.5 imagesize-0.7.1 pytz-2017.2 requests-2.17.3 six-1.10.0 
    snowballstemmer-1.2.1 sphinx-1.6.2 sphinxcontrib-websupport-1.0.1 urllib3-1.21.1
    ```

4. Install MyST-Parser extension

    This extension is required to parse Markdown-files. Install it with `pip`.

    ```
    C:\Users\michael>pip install myst-parser
    ...
    Successfully installed markdown-it-py-2.1.0 mdit-py-plugins-0.3.1 mdurl-0.1.2 myst-parser-0.18.1 pyyaml-6.0 typing-extensions-4.4.0
    ```

5. Install the official ReadTheDocsTheme (`sphinx_rtd_theme `)

    The official ReadTheDocs Theme can also be installed locally in order to test the build.

    ```
    C:\Users\michael>pip install sphinx_rtd_theme
    ...
    Successfully installed sphinx-rtd-theme-1.0.0
    ```    
6. Auto-reload

   There is a very useful extension that allow to host the documentation locally and refresh the browser on changes. 

   ```
    C:\Users\michael>pip install sphinx-autobuild
    ...
    Successfully installed sphinx-autobuild
    ```    

## Further information

- Installing Sphinx by the official spinx documentation on [http://www.sphinx-doc.org/en/stable/install.html](http://www.sphinx-doc.org/en/stable/install.html)
- Adding markdown support to sphinx from [http://blog.readthedocs.com/adding-markdown-support/](http://blog.readthedocs.com/adding-markdown-support/)
- Live reload extension from [https://github.com/GaretJax/sphinx-autobuild](https://github.com/GaretJax/sphinx-autobuild)