# idris2-jupyter-test

To open a notebook for an Idris2 project:

1) Install Python dependencies:
```
pip install jupyter pandas vega
pip install --upgrade notebook
jupyter nbextension install --user --py vega
pip3 install vega==3.5
```
2) Make sure to have the following in your global pack.toml file (generally found under ~/.pack/user/pack.toml):
```
[custom.all.json-schema]
type   = "github"
url    = "https://github.com/madman-bob/idris2-json-schema"
commit = "latest:main"
ipkg   = "json-schema.ipkg"
```
3) i) Make sure the local `pack.toml` of the project directory (in this case `vegatest`) contains the package paths:
  ```
  [custom.all.idris2-python]
  type        = "github"
  url         = "https://github.com/stefan-hoeck/idris2-python"
  commit      = "latest:pack"
  ipkg        = "idris2-python.ipkg"
  packagePath = true

  [custom.all.python]
  type   = "github"
  url    = "https://github.com/stefan-hoeck/idris2-python"
  commit = "latest:pack"
  ipkg   = "python-bindings.ipkg"

  [custom.all.jupyter]
  type   = "github"
  url    = "https://github.com/min-nguyen/idris2-jupyter"
  commit = "latest:pack-modular"
  ipkg   = "jupyter-bindings.ipkg"

  [custom.all.idris2-jupyter-vega]
  type   = "github"
  url    = "https://github.com/stefan-hoeck/idris2-jupyter-vega"
  commit = "latest:pack"
  ipkg   = "idris2-jupyter-vega.ipkg"
  ```
  ii) Make sure the local `.ipkg` of the project directory contains the dependencies `python, jupyter, idris2-jupyter-vega`.

4) Assuming you're in the directory `vegatest`, run the shell script in `../idris2-jupyter/run/jupyter_run.sh`.


The jupyter notebook will start an Idris REPL with `idris2 --find-ipkg` via a syscall. It will do so with the current working directory being the directory from where you started the jupyter notebook.  Idris needs an `.ipkg` file in scope (in this case `vegatest.ipkg`) listing all modules it must load into the REPL session; this must therefore go to the directory from which you start the jupyter notebook (or one of its parent directories), and it must list all module dependencies, otherwise, those modules will not be loaded and will not be found when you run stuff in the REPL in the notebook.

---

`IDRIS2_PACKAGE_PATH` is an environment variable listing all the directories where Idris should look for installed libraries. Pack generates a wrapper script at `$PACK_DIR/bin/idris2`, where it will invoke the idris2 executable with the `IDRIS2_PACKAGE_PATH` being correctly set (because pack installs Idris packages at custom locations). You can see what `IDRIS2_PACKAGE_PATH` Idris will get when being started from the jupyter notebook, by running `pack package_path` from the directory from where you also run the `jupyter_run.sh` script. If one of the modules listed in your `.ipkg` file is not part of the IDRIS2_PACKAGE_PATH, Idris will not find the module.

In order to make sure that the library in question will be found by Idris, it must be known to pack: Either because it is part of the official package collection, or because it is listed in a local `pack.toml` file, which must be in scope in the directory from where you run the `jupyter_run.sh` script, and not in the directory where the `jupyter_run.sh` script lies. But you must also install the dependencies before they are listed on the `IDRIS2_PACKAGE_PATH`. That's what `pack install-deps *.ipkg` does.
