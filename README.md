# Sphinx

Questions:
+ Using sphinx we can transform the docstrings inside the functions inside the Python modules into HTML files. Does it mean i do need have docstrings? While Sphinx is a powerful tool for generating documentation from docstrings in Python code, having docstrings is not strictly mandatory. However, using docstrings is considered a best practice in Python, and it's highly recommended to include them in your code.
+ If we don't have Python function or class it seems that doc is not generated? Like for my urlpatterns first of all i need to put some docstrings in the code to be able to get them in the generated doc html file and second since the urls.py file contains a list and not a function it is not documented I mean there is no source code shown in the html file, if I want to have the source code i need to put the urlpatterns in a function.
+ How to exclude some files from being in the HTML document generated? like the followings under countries package: 

        countries.migrations.0001_initial module
          Migration
            Migration.dependencies
            Migration.initial
            Migration.operations 


Workflow:

0- Create a directory called docs where all the documentation folders are stored
                
        cd root_directory_of_the_project
        mkdir docs
        
1- Sphinx comes with a script called **sphinx-quickstart** that sets up a source directory and creates a default conf.py with the most useful configuration values from a few questions it asks you. To use this, in ther terminal run:

        cd docs
        sphinx-quickstart

After running the sphinx-quickstart and answering a couple of questions, it creates a source directory with conf.py and a root document, index.rst. The main function of the root document, index.rst, is to serve as a welcome page, and to contain the root of the “table of contents tree” (or toctree). This is one of the main things that Sphinx adds to reStructuredText, a way to connect multiple files to a single hierarchy of documents. my index.rst:

        .. autodocs documentation master file, created by
           sphinx-quickstart on Mon Nov 27 13:59:12 2023.
           You can adapt this file completely to your liking, but it should at least
           contain the root `toctree` directive.
        
        Welcome to autodocs's documentation!
        ====================================
        
        .. toctree::
           :maxdepth: 2
           :caption: Contents:
        
           modules
        
        Indices and tables
        ==================
        
        * :ref:`genindex`
        * :ref:`modindex`
        * :ref:`search`

You can now create the files you listed in the toctree and add content, and their section titles will be inserted (up to the maxdepth level) at the place where the toctree directive is placed. Also, Sphinx now knows about the order and hierarchy of your documents. (They may contain toctree directives themselves, which means you can create deeply nested hierarchies if necessary.)

2- 

        cd .. # going to the main folder of the project 
        sphinx-apidoc -o docs . # I need to specify the name of the folder (docs) I want to have output in, also with dot I indicate that I want to have all the documentation for my current directory 

3- Make sure to include modules in the root document, index.rst.

4- The info in the conf.py file has been populated when we ran sphinx-quickstart and based on the answers we provided to the questions. We can modify them like:

        html_theme = 'sphinx_rtd_theme' # https://sphinx-themes.org/sample-sites/sphinx-rtd-theme/
        extensions = ["sphinx.ext.todo", "sphinx.ext.viewcode", "sphinx.ext.autodoc"] # https://www.sphinx-doc.org/en/master/usage/extensions/index.html

also, we need to get the absolute path of the **parent directory (..) of the current working directory** (which should be one level before docs directory which would be our project's main directory):

        import os
        import sys
        
        sys.path.insert(0, os.path.abspath("..")) 
        
+ sys.path: this is a list that contains directories where Python looks for modules to import. By default, it includes the current working directory and the standard library directories.
+ sys.path.insert(0, ...): this inserts the absolute path obtained from os.path.abspath("..") at the beginning (index 0) of the sys.path list. This means that Python will first look in the parent directory for modules before searching in other directories.

5- now we need to generate HTML files:

        cd docs # here we have make.bat file we want to run that with HTML argument because we want to generate HTML 
        .\make.bat html # this is for windows 
        sphinx-build -b html . _build/html # for wsl/ubuntu 

6- I got an error that seems to be related to the fact that Django is trying to access the INSTALLED_APPS setting, but the Django settings are not configured. This is likely happening because when Sphinx attempts to import the admin module from your Django application during the documentation build, it doesn't have the necessary Django settings configured. To solve it I revised my conf.py file as follows:

        import os
        import sys
        import django 
        
        sys.path.insert(0, os.path.abspath(".."))
        
        os.environ['DJANGO_SETTINGS_MODULE'] = 'worldCountries.settings'
        django.setup()

the useful link that suggests the above solution: https://www.freecodecamp.org/news/sphinx-for-django-documentation-2454e924b3bc/

7- Documenting objects: here  

+ 7.1 Class -- https://www.sphinx-doc.org/en/master/usage/domains/python.html#directive-py-class

  here
        
+ 7.2 methods -- https://www.sphinx-doc.org/en/master/usage/domains/python.html#directive-py-method 





get back to these:
problem of not recognizing some modules an submodules can be solved using: 
Run sphinx-apidoc with Recursive Option: Instead of running sphinx-apidoc -o docs ., try using the recursive option to make sure all subpackages and modules are included:
sphinx-apidoc -o docs . --separate --force 

cd docs
BUT still some of them has no codes or source, will get back to it later. 



+ sphinx-build -- https://www.sphinx-doc.org/en/master/man/sphinx-build.html

  sphinx-build [options] <sourcedir> <outputdir> [filenames …]


sphinx-build generates documentation from the files in sourcedir and places it in the outputdir.

sphinx-build looks for sourcedir/conf.py for the configuration settings. sphinx-quickstart(1) may be used to generate template files, including conf.py.

sphinx-build can create documentation in different formats. A format is selected by specifying the builder name on the command line; it defaults to HTML. Builders can also perform other tasks related to documentation processing. For a list of available builders, refer to Builders.

By default, everything that is outdated is built. Output only for selected files can be built by specifying individual filenames.



References:
https://sphinx-themes.org/

https://www.sphinx-doc.org/en/master/usage/quickstart.html:
