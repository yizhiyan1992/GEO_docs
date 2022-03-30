## Virtual Environment

1. `conda create --prefix ~/Desktop/folder/virtualenv/ python=3.7`:create virtual environment folder

2. `conda env list`: check all virtual environments

3. `conda activate <virtual_env>`: switch into virtual env

4. `conda deactivate`: quit from virtual env

5. `which pip`: check the pip that is currently using

6. `conda install nb_conda` : use virtual env in jupyter notebook

## How to connect jupyter in remote desktop at local host?

- step 1: install jypyter notebook in remote desktop, and add it into PATH.

- step 2:

`jupyter notebook --generate-config` :generate config file, the default place is at ~/.jupyter/jupyter_notebook_config.py

`jupyter notebook password` : set a password for future login.

`vim ~/.jupyter/jupyter_notebook_config.json` : find the hash for password

`vim ~/.jupyter/jupyter_notebook_config.py` : open it to change the following configurations:

```
c.NotebookApp.ip='*' # all IPs can be accessed
c.NotebookApp.password=<hash_val> # paste the hash value here
c.NotebookApp.open_browser=False # do not open the browser by default
c.NotebookApp.port=8888 # assign the port number
c.NotebookApp.allow_remote_access=True # allow remote access
```

- step 3: `jupyter notebook --ip=0.0.0.0 --no-browser --allow-root` : open notebook. If using virtual env, activete it first

- step 4: access through <http://ip:port/> or <http://localhost:port>
