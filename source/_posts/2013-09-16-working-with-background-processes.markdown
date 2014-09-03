---
layout: post
title: "Working with Background Processes"
date: 2013-09-16 21:33
updated: 2013-09-16 21:33
comments: true
categories: unix
---

For long running processes, rather than blocking your prompt, it's often useful to push commands to the background and complete other tasks.

To run a command in the background.

	<command> &

To run a command in the background, detached from your console.  The process will not terminate on logout.  `nice` sets a lower priority.

	nohup nice <command> &

List all background processes

	jobs

To send currently running command to background, first stop the process, `Ctrl-z`.

	bg

To bring a background process to the foreground

	fg
	fg %1

To destroy a background process

	kill %1	
	kill -9 %1	
	kill <pid>

To receive an email notification when a background process finishes

	<command> | tee command.log | mailx -s 'PROCESS COMPLETE' test@example.com &

To run a set of jobs when cpu levels permit, use batch.  <kbd>ctrl</kbd>+<kbd>d</kbd> to end input.

	user@example.com > ~/.forward
	batch -m
	command1
	command2
	command3

You can list batch jobs and kill using the id

	at -l
	at -r id

