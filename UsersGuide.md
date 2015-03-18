# Introduction #

libcli provides a consistant command-line environment for remote clients, with a few common features between every implemtation.

The library is not accessed by itself, rather the software which uses it listens on a defined port for a Telnet connection. This connection is handed off to libcli for processing.

The first thing that libcli does is the Telnet negotiation necessary to establish a character mode session, rather than the Telnet default of a line mode session. This is to enable command-line editing, completion and history.

A libcli implementation may require authentication before giving you access to the environment. If so, a _Username:_ prompt will be issued. Enter the username you have been given, and a _Password:_ prompt will appear. The characters you enter for your password will not be echoed.

To leave any libcli command-line environment, enter the command "_quit_" (aliases are _exit_ and _logout_), hit Ctrl-D, or simply break the connection.

# History #

libcli keeps track of the last 256 commands you entered in the session.  To navigate through the history, use the up and down arrow keys (or `^P`/`^N`).

You can also enter "_history_" to get a list of all the commands in the history.

# Command-Line Editing #

You can edit the command currently at the prompt:
  * Left and right arrows move the cursor around on the line, as do `^B`/`^F`.
  * `^A` moves the cursor to the start of line, `^E` to the end.
  * `^H` and DEL delete the character to the left of the cursor.
  * `^W` deletes the word to the left of the cursor.
  * `^U` clears the current line.

After changing the line and hitting enter, the new command line will be added to the end of the history.

If you don't remember the command name that you want, you can press ? at any time to get a list of available commands. If you enter ? when you are half-way through entering a word, you will get a list of all commands which match what you have already entered.

# Filters #

You can limit the output of any command to a subset of the total output by using any of the following filters.

You specify the filters you want to use by appending **|** (pipe) to your command line, followed by the filter name, and any parameters that the filter requires.  Parameters may be quoted with '' or "".  If more parameters are provided than are expected by the filter, additional arguments are appended to the last, seperated by a single space (i.e. "`| inc foo   bar`" is equivalent to "`| inc 'foo bar'`").

## Available Filters ##

### `i[nclude]` _string_ ###
### `ex[clude]` _string_ ###
Include or exclude lines which contain the literal string given by _string_.

### `beg[in]` _string_ ###
Include all lines from the first which matches the given _string_.

### `bet[ween]` _string1_ _string2_ ###
Include lines which include _string1_ through to the next line which matches _string2_.

### `c[ount]` ###
A count of non-blank lines is output.

### `g[rep]` [-vie] _pattern_ ###
### `eg[rep]` [-vie] _pattern_ ###
Include lines which match the regular expression (or extended regular expression) given by _pattern_.

The `-i` option makes the match case insensitive, `-v` inverts the sense of the test (include lines which do not match) and `-e` may be used on the off chance you wish to search for a string matcing `^-[vie]+$`