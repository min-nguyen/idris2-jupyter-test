# idris2-jupyter-test


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
3) Assuming you're in the root directory, run the shell script in `idris2-jupyter/run` and provide the target Idris project as a path:
```
./idris2-jupyter/run/jupyter_run.sh vegatest
```
