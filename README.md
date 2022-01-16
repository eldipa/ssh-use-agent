
# Install

Copy the script to /usr/bin, your home or other place

```shell
$ cp ssh-use-agent ~/your-scripts/
```

Make sure it has the correct permissions

```shell
$ chmod 0744 ~/your-scripts/ssh-use-agent
```

Optionally, but **highly recommended**, add an ``alias`` into your ``~/.bashrc``
or other similar depending of your shell.

```shell
alias ssh-use-agent=". ~/your-scripts/ssh-use-agent"
```

Notice the "dot" notation to source *into* your current shell: this is needed
to configure the shell that is executing the script.

Without this, you need to explicitly call ``source`` or use the "dot"
notation.

# Use

If you have the script installed and with the ``alias`` configured,
to use a ``ssh-agent`` run:

```shell
$ ssh-use-agent use <cool-name>
```

If the ``ssh-agent`` under the ``<cool-name>`` does not exist, it will
be created *without* any key loaded.

To know which ``ssh-agent`` is loaded in the current shell run:

```shell
$ ssh-use-agent current
<cool-name>
```
In other way you could retrieve also information as this :

```shell
$ echo $SSH_AGENT_NAME
<cool-name>

$ echo $SSH_AGENT_PID
<pid>

$ echo $SSH_AUTH_SOCK
/tmp/<...>
```

To load a key into the current ``ssh-agent``, run ``ssh-add`` as usual:

```shell
$ ssh-add ~/.ssh/<your-key>
```

To list what keys are loaded run ``ssh-add -l`` as usual:

```shell
$ ssh-add -l
```

You can use the *same* ``ssh-agent`` in another terminal running (where
``<cool-name>`` is the same name as before):

```shell
$ ssh-use-agent use <cool-name>
```

Because it is about the *same* ``ssh-agent``, the keys are already loaded.

To use *another* ``ssh-agent``, run the same command with a different name:

```shell
$ ssh-use-agent use <another-cool-name>
```

The first ``ssh-agent`` will be still running: now there are 2 *separated*
``ssh-agent`` instances running.

To unset the ``ssh-agent`` in the current terminal run:

```shell
$ ssh-use-agent disuse
```

You can verify that the current shell has no ``ssh-agent`` configured

```shell
$ echo $SSH_AGENT_NAME
```

But, the ``ssh-agent`` instances are **not** killed: they are still alive
and can be activated again in the current shell.

If you want to remove a key from a ``ssh-agent``, use ``ssh-add`` as usual:

```shell
$ ssh-use-agent use <cool-name>
$ ssh-add -d ~/.ssh/<your-key>
```

Or use ``ssh-add -D`` to remove all the keys; use ``kill -15 $SSH_AGENT_PID``
to kill the ``ssh-agent`` instance.

