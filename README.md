# Sphinx

https://sphinx-themes.org/

Some notes from https://www.sphinx-doc.org/en/master/usage/quickstart.html:
+ Let’s assume you’ve run sphinx-quickstart. It created a source directory with conf.py and a root document, index.rst. The main function of the root document is to serve as a welcome page, and to contain the root of the “table of contents tree” (or toctree). This is one of the main things that Sphinx adds to reStructuredText, a way to connect multiple files to a single hierarchy of documents. my index.rst:

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

+ sphinx-build -- https://www.sphinx-doc.org/en/master/man/sphinx-build.html

  sphinx-build [options] <sourcedir> <outputdir> [filenames …]


sphinx-build generates documentation from the files in sourcedir and places it in the outputdir.

sphinx-build looks for sourcedir/conf.py for the configuration settings. sphinx-quickstart(1) may be used to generate template files, including conf.py.

sphinx-build can create documentation in different formats. A format is selected by specifying the builder name on the command line; it defaults to HTML. Builders can also perform other tasks related to documentation processing. For a list of available builders, refer to Builders.

By default, everything that is outdated is built. Output only for selected files can be built by specifying individual filenames.

+ Documenting objects:

        + Class -- https://www.sphinx-doc.org/en/master/usage/domains/python.html#directive-py-class
        
        + methods -- https://www.sphinx-doc.org/en/master/usage/domains/python.html#directive-py-method 
