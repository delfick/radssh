Frequently Asked Questions
==========================

RadSSH Project
--------------

#. What is RadSSH?

   RadSSH is a Python package that works with the Paramiko SSH package to implement a parallel shell-like environment for clusters of remote hosts using the SSH protocol.

#. How do I install RadSSH?

   The preferred installation method is using the Python Installation Program, **pip**, and by the time you are reading this, RadSSH should be registered and uploaded to the Python Package Index, PyPI.

#. What version(s) of Python does RadSSH work with?

   RadSSH is primarily developed and tested under Python 2.6/2.7. It functions and has been tested to a lesser extent under Python 3.3. **pip** installation will convert the source using the Python **2to3** source conversion utility. The goal is to support any Python runtime version that is supported by Paramiko.

#. Does RadSSH work on Windows systems?

   Not currently, but it is being pursued for a future release. RadSSH can run on just about any Linux distribution, as well as recent Macintosh OS X releases. It has only been tested on Windows to the point of revealing that some non-trivial code changes are needed to support Windows and/or Cygwin.

   If you cannot install RadSSH on one of the SSH destination hosts that you connect to from Windows, a virtual machine installation of a Linux distribution can be used.

#. What remote hosts can RadSSH connect to?

   In addition to Linux, Macintosh, and Solaris hosts, RadSSH has also been confirmed to some Linux based appliances, as well as Cisco IOS switches. Cisco switch connections, and possibly other hosts, may need to explicitly change some configuration settings, such as **force_tty** and **try_auth_none**.

RadSSH Shell
------------

#. Can I use wildcard or hostname expansion, instead of listing each host individually?

   Not directly. You can leverage Bash to do some degree of expansion, either using braces (**192.168.100.{1..50}**, or **webfarm_{001..025}**), or you can provide a filename that is a text file with hosts (or IP addresses) listed one entry per line. More elaborate expansion and translation schemes are able to be implemented by custom plugins lookup() functions.

#. How do I change my configuration settings?
   All RadSSH configuration settings can be set either from the command line, or from a saved configuration settings file (``~/.radssh_config``). On the command line, use the form **--keyword=value**; in the configuration file, use **keyword=value**. To see the default configuration settings, you can run ``python -m radssh.config``.

#. Do I need to create a .radssh_config settings file?

   It is not manatory to have a ``~/.radssh_config`` file. Any settings that could be set in a config file can also be specified on the command line, via **--keyword=value** parameters. If you consistently use the same settings on the command line, it may be more convenient to save them in a config file.

#. Can I change the username that RadSSH uses for login?

   By default, the local $USER environment variable is used. This can be overridden by setting a **username** in the configuration file '~/.radssh_config'. or on the command line with **--username=newname**

#. Why doesn't **exit** work?

   It probably is working, it just does not behave the way you expect it to.

   The exit command line is actually being run on all the remote hosts. With no additional arguments, it produces no output and provides a status code of zero, indicating success. RadSSH issues the command just like any other, and reports the output (zero bytes) to the console, and reports the summary of errors (none).

   Try running **exit 1** or **exit $RANDOM** to see RadSSH reporting non-zero status codes.

   If you actually want to exit the RadSSH session, you can use **\*exit** or <Ctrl-D>

#. I need to run some commands with **sudo**, and it isn't working.

   If you need to enter your own password in order to invoke sudo, you are currently out of luck. RadSSH is not currently able to handle interactive prompting. See if the sudo configuration option NOPASSWD is available.

   Most sudo configurations also, by default, require a TTY, which is also not established for RadSSH connections. Running commands with **sudo** under RadSSH requires both NOPASSWD and !requiretty.
   
#. I keep getting "Unable to verify host key" messages, and can't connect to hosts. Why is this?

   By default, RadSSH will not interact with hosts that are not already known and trusted. SSH command line clients use ~/.ssh/known_hosts to keep a record of hosts with verified and trusted keys. If RadSSH refuses to communicate with a host, make sure that you can connect with your normal ssh client. You can also turn on automatic accepting of host keys with **--hostkey.verify=accept_new**, which sacrifices some security in exchange for some convenience.

#. Plugins are failing to load.

   Plugins are used to add functionality to the RadSSH shell, but the core functionality is not dependent on any plugins. Failing to load plugins is treated as a warning, not an outright error. If you, the user, are not dependent on the functionality of any of the failed plugins, you can safely ignore the warning(s).

   Most plugin load failures arise from not having a needed Python module for the plugin. The core plugins for RadSSH should only need modules from the Python Standard Library, but supplemental and third-party plugins are free to rely on other modules, which may or may not be available.

   To see details on which plugins are failing to load, and the specific load errors, include **--verbose=on** option. You can install the needed modules that the plugin depends on, or if you have no use for the functionality of the plugin, you can add the plugin name to the **disable_plugins** list.

User Authentication
-------------------

#. Can I use SSH keys to authenticate instead of passwords?

   Yes. You will need to indicate to RadSSH that you want to use identity keyfiles (~/.ssh/id_rsa & ~/.ssh/id_dsa) with **--ssh-identity=on**. RadSSH can also use keys available via SSH Agent, by enabling the option **--ssh-agent=on**. Both of these configuration options are off by default.

   You can also configure RadSSH to use keyfiles from other locations, through use of an AuthFile, which enables you to specify key files and passwords used for authentication.

#. Can I set a saved password in ~/.radssh_config?

   No. It is a bad idea to have a settings file contain sensitive information, like a password in plain text.  If you want to save passwords in a file, you should probably look into setting up an AuthFile.

#. Can I set a saved password in an AuthFile?

   Yes. In addition, you should probably use the radssh.pkcs utility to save the password(s) in encrypted form rather than plain text.
