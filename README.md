# wait_for_process
Simple utility to wait for matching running processes to finish and exit. 

## INTRODUCTION

If you have one or more processes running on the same machine but outside of
your current tty this program provides a convenient way to wait for these other
processes to finish before executing another command. For example, if we are
running one or more instances of "analysis_command" that generates data we want
to compress once completed, we might run something like the following:

    wait_for_process -p analysis_command && tar -cjvf output.tar.bz2 /path/to/output_folder

## REQUIREMENTS
This program is designed to work on **Linux** systems - it relies on the `pgrep`
command and on the `/proc` folder. It is written in **perl** and should not require
any additional software or modules other than that included in a most standard perl
distributions bundled with your OS.

## INSTALLATION
Clone this repo:

    git clone https://github.com/david-a-parry/wait_for_process.git

Either run directly from the cloned repo:

    ./wait_for_process/wait_for_process [options]
    
Or add to/copy to somewhere in your PATH and invoke directly - e.g.

    cp ./wait_for_process/wait_for_process $HOME/bin
    wait_for_process [options]

## BACKGROUND

A simple way to wait for a process to end without this script is the following:

    tail --pid=${pid} -f /dev/null

Which works fine if you know the processes PID already. This program provides
some convenience functions including:

* letting you specifying the process name(s) that you want to wait for
* letting you use any part of the full process name(s) that you want to wait for
* letting you continuously check for matching processes and wait for any newly spawned matching process

See the usage below for more details.


## SYNOPSIS

        wait_for_process [program name]
        wait_for_process -p [program name]
        wait_for_process -f [pattern in full command line]
        wait_for_process -u [user]
        wait_for_process -i [pid]
        wait_for_process -h (display help message)
        wait_for_process -m (display manual page)

## ARGUMENTS

- **-p    --process**

    One or more programs to check processes for.

- **-f    --full**

    One or more strings to search in full commands.

- **-u    --user**

    One or more users to restrict search to.  If used without --process or --full arguments will search all users processes (including logins!).

- **-i    --id**

    One or more specific PIDs to check regardless of other options.

- **-d    --delay**

    Delay in seconds between checks.  Defaults to 10.

- **-c    --continuous**

    Continuously check for new processes matching your criteria and wait for these to finish too.

- **-q    --quiet**

    Suppress printing of matching/finished PIDs.

- **-n    --non\_interactive**

    Use this if for some reason you don't want to be able to exit the program by pressing the 'Q' key. 

- **-h    --help**

    Display help message and exit.

- **-m    --manual**

    Show manual page and exit.

## EXAMPLES

- `wait_for_process -p cat`

    _wait for all 'cat' processes to finish_

- `wait_for_process cat`

    _as above, command line arguments are assumed to be processes if not preceded by one of the above argument categories_

- `wait_for_process -p cat -u bob`

    _wait for all 'cat' processes being run by user 'bob' to finish_

- `wait_for_process -u bob`

    _wait for all processes being run by user 'bob', including logins, to finish_

- `wait_for_process -f foo -c`

    _wait for all commands containing the word 'foo' to finish, including processes spawning while this program is running_

- `wait_for_process -p cat -f foo`

    _wait for all 'cat' processes AND all commands containing the word 'foo' to finish_

- `wait_for_process -p cat -f foo -u bob`

    _wait for all 'cat' processes by user 'bob' AND all commands containing the word 'foo' by user 'bob' to finish_

- `wait_for_process -i 1234`

    _wait for process 1234 to finish_

## DESCRIPTION

This program searches for running processes that match the provided arguments
and waits until they have finished before exiting. It can be used to queue a
command you want to run once one or more other processes in another session have
finished.

e.g. wait\_for\_process -p \[command generating big\_text\_file.txt\] ; gzip -9 big\_text\_file.txt

You can exit manually at any time by pressing 'Q' (unless using the
--non\_interactive option).

Please note - this program only searches for processes running at the time of
its execution unless using the --continuous flag.

# AUTHOR

David A. Parry

# COPYRIGHT AND LICENSE

Copyright 2021  David A. Parry

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version. This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more
details. You should have received a copy of the GNU General Public License along
with this program. If not, see &lt;http://www.gnu.org/licenses/>.
