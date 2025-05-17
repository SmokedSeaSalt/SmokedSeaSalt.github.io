---
title: ft_printf
description: My ft_printf notes.
author: Mathijs
date: 2025-05-12 09:43:00 +0100
categories: [CODAM]
tags: []
pin: false
math: false
mermaid: true
---

## get_next_line
This will just be a thought collection or code snippet storage. Not a detailed walkthrough or example of ft_printf.

Write a function that returns a line read from a
file descriptor

## overview

- Repeated calls (e.g., using a loop) to your get_next_line() function should let you read the text file pointed to by the file descriptor, one line at a time.
- Your function should return the line that was read. If there is nothing left to read or if an error occurs, it should return NULL.
- Make sure that your function works as expected both when reading a file and when reading from the standard input.
- Please note that the returned line should include the terminating \n character, except when the end of the file is reached and the file does not end with a \n character.
- Your header file get_next_line.h must at least contain the prototype of the get_next_line() function.
- Add all the helper functions you need in the get_next_line_utils.c file.
- Because you will have to read files in get_next_line(), add this option to your compiler call: -D BUFFER_SIZE=n It will define the buffer size for read(). The buffer size value will be adjusted by your peer evaluators and the Moulinette to test your code.
- You will compile your code as follows (a buffer size of 42 is used as an example): cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 <files>.c
- get_next_line() exhibits undefined behavior if the file associated with the file descriptor is modified after the last call, while read() has not yet reached the end of the file.
- get_next_line() also exhibits undefined behavior when reading a binary file. How- ever, you can implement a logical way to handle this behavior if you want to.

### Forbidden

- You are not allowed to use your libft in this project.
- lseek() is forbidden.
- Global variables are forbidden.

## Bonus

This project is straightforward and does not support complex bonus features. However, we trust your creativity. If you have completed the mandatory part, consider attempting this bonus section.

Here are the bonus part requirements:
- Develop get_next_line() using only one static variable.
- Your get_next_line() can manage multiple file descriptors at the same time.
For example, if you are reading from file descriptors 3, 4, and 5, you should be able to read from a different file descriptor with each call, without losing track of the reading state of each file descriptor or returning a line from a different one. This means you should be able to call get_next_line() to read from fd 3, then fd 4, then fd 5, then again from fd 3, then fd 4, and so forth, without losing track of the reading state for each file descriptor. Append the _bonus.[c\h] suffix to the bonus part files. It means that, in addition to the mandatory part files, you will turn in the 3 following files:
- get_next_line_bonus.c
- get_next_line_bonus.h
- get_next_line_utils_bonus.c

## My implementation

To comply with the 1 static rule I made a Linked list with the head static.
With this I can create a list entry for every fd.
