
# milatools

The milatools package provides the `mila` command, which is meant to help with connecting to and interacting with the Mila cluster.


## Install

Requires Python >= 3.8

```bash
pip install milatools
```

Or, for bleeding edge version:

```bash
pip install git+https://github.com/mila-iqia/milatools.git
```

After installing `milatools`, start with `mila init`:

```bash
mila init
```


## Commands

### mila init

Set up your access to the mila cluster interactively. Have your username and password ready!

* Set up your SSH config for easy connection with `ssh mila`
* Set up your public key if you don't already have them
* Copy your public key over to the cluster for passwordless auth
* Set up a public key on the login node to enable ssh into compute nodes


### mila docs/intranet

* Use `mila docs <search terms>` to search the Mila technical documentation
* Use `mila intranet <search terms>` to search the Mila intranet

Both commands open a browser window. If no search terms are given you are taken to the home page.


### mila code

Connect a VSCode instance to a compute node. `mila code` first allocates a compute node using slurm (you can pass slurm options as well using `--alloc`), and then calls the `code` command with the appropriate options to start a remote coding session on the allocated node.

You can simply Ctrl+C the process to end the session.

```
usage: mila code [-h] [--alloc ...] [--job VALUE] [--node VALUE] PATH

positional arguments:
  PATH          Path to open on the remote machine

optional arguments:
  -h, --help    show this help message and exit
  --alloc ...   Extra options to pass to slurm
  --job VALUE   Job ID to connect to
  --node VALUE  Node to connect to
```

For example:

```bash
mila code path/to/my/experiment
```

The `--alloc` option may be used to pass extra arguments to `salloc` when allocating a node (for example, `--alloc --gres=cpu:8` to allocate 8 CPUs). `--alloc` should be at the end, because it will take all of the arguments that come after it.

If you already have an allocation on a compute node, you may use the `--node NODENAME` or `--job JOBID` options to connect to that node.


### mila serve

The purpose of `mila serve` is to make it easier to start notebooks, logging servers, etc. on the compute nodes and connect to them.

```
usage: mila serve [-h] {connect,kill,list,lab,notebook,tensorboard,mlflow,aim} ...

positional arguments:
  {connect,kill,list,lab,notebook,tensorboard,mlflow,aim}
    connect             Reconnect to a persistent server.
    kill                Kill a persistent server.
    list                List active servers.
    lab                 Start a Jupyterlab server.
    notebook            Start a Jupyter Notebook server.
    tensorboard         Start a Tensorboard server.
    mlflow              Start an MLFlow server.
    aim                 Start an AIM server.

optional arguments:
  -h, --help            show this help message and exit
```

For example, to start jupyterlab with one GPU, you may write:

```bash
mila serve lab --alloc --gres gpu:1
```

You can of course write any SLURM arguments after `--alloc`.

Ending the connection will end the server, but the `--persist` flag can be used to prevent that. In that case you would be able to write `mila serve connect jupyter-lab` in order to reconnect to your running instance. Use `mila serve list` and `mila serve kill` to view and manage any running instances.
