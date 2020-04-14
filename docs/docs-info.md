# Docs information

On this page some information is provided how to operate the docs.
For complete information see: https://www.mkdocs.org/user-guide/writing-your-docs/

!!! tip
    It is possible to create pages and edit the configuration from GitLab directly **BUT** it is much more convenient to run it locally in case of problems.
    This can be done by cloning the repository:
    ```shell
      git clone https://gitlab.cern.ch/be-bi-bgi/bgi-documentation.git && cd bgi-documentation
    ```
    To run locally you need to install MkDocs and the theme.
    In case of problems check your Python version `python --version`.
    It has been tested and working on MacOS 10.15.1 using Python 3.7.4.
    ```shell
      pip install mkdocs mkdocs-material
    ```
    Now you can hopefully start MkDocs and see the website at http://127.0.0.1:8000/ using the command below (see https://www.mkdocs.org/#getting-started for more info).
    ```shell
      mkdocs serve
    ```
    As soon as you edit a page or the config file, the website will reload.
    If there are any errors it might not reload and you need to check the output in the terminal.
    When you are done, commit the changes and push to the repository ([Git instructions](https://git-scm.com/book/en/v2))
    The public website will reload automatically after a minute or two.


## Creating new pages
Creating a new page involves two steps.

1. Create the .md file in the correct directory
1. Update the mkdocs.yml file

??? info "Directory Structure"
    The directory structure should be kept the same as the structure of the website.
    ```shell
      |-electronics
      |---detector
      |---readout
      |-mechanics
      |-physics
      |-software
      |---analysis
      |---lab-debug
      |---operational
    ```

!!! example
    The important section of the mkdocs.yml file is under `nav`.
    Indentation level sets the structure of the pages.
    This should match the directory structure.
    A snippet of the file can be seen below:
    ```yaml
      nav:
        - Home:
          - Welcome: index.md
          - Docs information: docs-info.md
        - Software:
          - Overview: software/overview.md
          - Lab & Debug:
            - Panda GUI: software/lab-debug/panda-gui.md
    ```

## Create a Markdown page from a Jupyter notebook
This will include figures and tables from the compiled notebook.
More info at: https://predictablynoisy.com/jekyll-markdown-nbconvert

!!! snippet
    ```shell
    !jupyter nbconvert --to markdown --output-dir='OUTPUT_DIRECTORY' NAME_OF_THE_NOTEBOOK.ipynb
    ```

### Extending the default template
See: https://nbconvert.readthedocs.io/en/latest/customizing.html#Custom-Templates

