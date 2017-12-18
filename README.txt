Qdb: a really simple remote client/server debugger for Python

What?

Python Bdb (stdlib) based debugger
Remote: attachable from external processes (even on other machines)
Client/Server: a backend designed to run with several frontends (wx, web2py, etc.)
Simple & Small: just a enhanced Pdb
Why?

pdb is text only (and local process), this causes big troubles with wx and web servers
rpdb2 (winpdb) is too complex to use/extend in my case (rpdb2 is 14500 LOC against ~700 LOC of this script)
I need this for a totally integrated IDE I'm writing (see rad2py)
IPC/RPC Flexibility: supports Threading Queues, Multiprocessing Pipes and Sockets
Cross-Version (py2.6+ & py3k), Cross-Platform (win/linux/mac interoperability) (work in progress)
How?

bdb: Python Debugger framework
multiprocessing: listeners/clients: queue/pipe communication (serialization w/ pickle) requires python 2.6+
JSON-RPC: JSON-like Remote Procedure Call protocol over stream connections (see also simplejsonrpc.py in this project)
Cmd: cmd — Support for line-oriented command interpreters
wxpython: toolkit for visual interface
Download

Just one file: qdb.py (download raw file)

Commands

The Command Line Interface accepts is very similar to pdb:

c: Continue execution, only stop when a breakpoint is encountered.
s: Execute the current line, stop at the first possible occasion
n: Execute the current line, do not stop at function calls
r: Continue execution until the current function returns
p: Inspect the value of the expression
w: Print a stack trace, with the most recent frame at the bottom.
l [from:to]: List source code for the current file
j lineno: Set the next line that will be executed.
b filename:lineno: Set a breakpoint at filename:breakpoint
h: help
q: Quit from the debugger. The program being executed is aborted.
For more information, see: pdb debugger commands

Usage

Standalone

First, ensure you have qdb.py installed in your python path (or in the current directory).

The call the python interpreter with the option -m qdb and your script name. This will load your program under qdb debugging.

Sample Session in the backend:

reingart@desktop:~/rad2py/ide2py$ python -m qdb hola.py qdb debugger backend: waiting for connection at ('localhost', 6000) qdb debugger backend: connected to ('127.0.0.1', 52322)

Then, in other terminal, run the python interpreter with the option -m qdb only. This will launch the qdb attached to the debugging session.

Sample session in the frontend (Command Line Interface): ``` reingart@desktop:~/rad2py/ide2py$ python -m qdb qdb debugger fronted: waiting for connection to ('localhost', 6000) running hola.py

hola.py(3) -> name = raw_input("please write your name:") (Cmd) n please write your name: mariano hola.py(4) -> print "hello", name (Cmd) !name = 'Mariano' None hola.py(4) -> print "hello", name (Cmd) n hello Mariano hola.py(6) -> def factorial(n): (Cmd) l hola.py: 1 # sample file to test shell & debugger hola.py: 2 hola.py: 3 name = raw_input("please write your name:") hola.py: 4 print "hello", name hola.py: 5 hola.py: 6-> def factorial(n): hola.py: 7 """This is the "factorial" function hola.py: 8 >>> factorial(5) hola.py: 9 120 hola.py: 10 hola.py: 11 """ ```

Note: (Cmd) is the prompt, just type your command (i.e. n) and hit enter.
