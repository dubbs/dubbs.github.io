---
layout: post
title: "Unix Pipelines vs Redirection"
date: 2013-09-25 20:05
updated: 2013-09-25 20:05
comments: true
categories: unix
---

Redirection is used to send data from standard streams to specific locations.

To send the standard output stream to a file, instead of the terminal:

	command1 > outfile
	command1 1> outfile

Same as above, but instead send the standard error stream:

	command1 2> outfile

To send standard output/error streams to a file, instead of the terminal:

	command1 > outfile 2>&1

Output can also be disposed of using the null device:

	command1 > /dev/null 2>&1

To use the contents of a file as the standard input stream to a command, instead of using keyboard input:

	command1 < infile

Input can be read from one file and output to another:

	command1 < infile > outfile

The standard output of one command can also be used as the standard input to another using a temporary file:

	command1 > file
	command2 < file
	rm file

However, this is inefficient as the second command has to wait for the first to complete before proceeding.  Also, there is a chance that the temporary file will overwrite an already existing one.

Instead, it is more efficient to directly stream the output of one command into another via `pipes`:

	command1 | command2

Along with the standard out, you can also send standard error, notice that it appears before the pipe.

	command1 2>&1 | command2

It is also possible to direct the output of a command to standard out and an outfile using `tee`.

	command1 | tee outfile

Lastly, if you want to avoid overwriting files when redirecting, set noclobber:

	set -o noclobber
	command1 > existingfile
	# -bash: existingfile: cannot overwrite existing file

