# Altair and JupyterLab setup
​
[Altair](https://altair-viz.github.io/) is a declarative statistical visualization library for Python, based on [Vega](http://vega.github.io/vega) and [Vega-Lite](http://vega.github.io/vega-lite).
We will use it in class so we want to get set up with it on your laptop.

If you run into problems see the [Troubleshooting](#Troubleshooting) below and look at the source code and documentation  for Altair on [GitHub](http://github.com/altair-viz/altair).

# Setup instructions

1. Clone the repo.
2. `CD` to the repo directory. Create and activate a virtual environment for this project. You may need to modify the code you use depending on what Python you have installed and how your machine is configured.
  * On macOS or Linux:
    ```
    python3 -m venv env
    source env/bin/activate
    which python
    ```
  * On Windows:
    ```
    python -m venv env
    .\env\Scripts\activate
    where python
    ```
3. Install necessary packages. Note that you have to install the exact versions of the packages.
    ```
    pip install -r requirements.txt
    ```

# Run instructions

1. Run `jupyter lab`. It should open your browser and let you select select any Jupyter Notebook .ipynb file.
2. Run individual cells with `ctrl+enter`. In the menu you can run all cells and restart the kernel to clear variables.
​
# Quit instructions
1. Make sure to save your .ipynb file and shutdown Jupyter Lab properly through the file menu. Otherwise you need to use `jupyter notebook stop`.
​
2. Deactivate the venv to return to your terminal using `deactivate`.

# Directions for committing

1. If you have made any changes to the required packages you should export a list of all installed packages and their versions:
   ```
   pip freeze > requirements.txt
   ```

2. **Before you commit a Jupyter Notebook .ipynb file, clear the outputs of all cells.** This decreases file size, removes unnecessary metadata, and makes diffs easier to understand. In Jupyter Lab you can use the GUI: Edit->Clear All Outputs.

## Optional git setup to automatically clear metadata using JQ (Highly Recommended)

1. Install JQ by running `sudo apt-get install jq`. For more options check [here](https://stedolan.github.io/jq/download/).
2. Append the following block of code either in your local repo `.gitconfig` file or your global `.gitconfig`. I would recommend to do it in your global `.gitconfig` so you don't need to redo that for future .ipynb files.<br>
    ```
    [core]
    attributesfile = ~/.gitattributes_global
    [filter "nbstrip_full"]
    clean = "jq --indent 1 \
            '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
            | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
            | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
            | .cells[].metadata = {} \
            '"
    smudge = cat
    required = true
    ```
    For more details look at this great tutorial [here](http://timstaley.co.uk/posts/making-git-and-jupyter-notebooks-play-nice/).
3. Create a global gitattributes named `.gitattributes_global` file (usually placed at the root level, so `~/.gitattributes_global`).
4.  Add the following line of code:
    ```
    *.ipynb filter=nbstrip_full
    ```

# Optional features

* To get markdown section numbering use [jupyterlab-toc](https://github.com/jupyterlab/jupyterlab-toc).
  * To install run: `jupyter labextension install @jupyterlab/toc`
  * Then click the enumerated list button on the left strip in JupyterLab to bring up the table of contents. There you can click the itemized list button in the top to add section numbers to the markdown cells.
​
* There is also a useful [Spellchecker](https://github.com/ijmbarr/jupyterlab_spellchecker) extension.
  *  To install run: `jupyter labextension install @ijmbarr/jupyterlab_spellchecker`

* To install both of the above in one command run: `jupyter labextension install @jupyterlab/toc @ijmbarr/jupyterlab_spellchecker`
​
# Troubleshooting

* If you get a `NotImplementedError` for `asyncio` while running Python 3.8,
edit `/env/Lib/site-packages/tornado/platform/asyncio.py` following the instructions [here](https://stackoverflow.com/questions/58422817/jupyter-notebook-with-python-3-8-notimplementederror/). Right after the line `import asyncio` add these lines:

```
import sys
if sys.platform == 'win32':
    asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
```

* You may run into issues where pip is using a different python than Jupyter Lab is running. E.g., you may install a package but then Jupyter complains it is unavailable. In that case, instead of `pip install` try running `python -m pip install`.

* You may have trouble using Python and Jupyter Lab with the [Anaconda Distribution](https://www.anaconda.com/distribution/). In that case, one fix is to uninstall Anaconda completely and install basic [Python](https://www.python.org/downloads/).

* The developers of Altair sometimes release a new version via pip and update the documentation before it is stable.
    Note that the documentation version and altair version you are using may be out of sync.
    In this assignment, using the `requirements.txt` file we are asking you to install a particular version to avoid some issues like this.