```
sudo -s
pip install jupyter
pip install jupyterlab
pip install jupyterlab-git
apt install supervisor
```

Create normal user. Run the following as normal user, not root:

```
jupyter lab --generate-config
jupyter lab password
```

Edit config file at `~/.jupyter/jupyter_lab_config.py`

```
c.LabServerApp.open_browser = False
c.ServerApp.ip = '*'
c.ServerApp.port = 9999
```

Add to `/etc/supervisor/supervisor.conf`

```
[program:jupyter]
directory=/home/pingswept
environment=HOME="/home/pingswept",USER="pingswept"
command=jupyter lab
user=pingswept
```
