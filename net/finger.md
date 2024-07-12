
# Finger (RFC 742, RFC 1288)

The Finger protocol is extremely simple. It was created in 1971 by Les Earnest
as a way to get information about who was logged in on a remote server, and
and what they were working on. By the late 1990s it had gone out of favor as it
had been superceded by more secure protocols and the World Wide Web.

[Here](https://gist.github.com/erysdren/76543b72935ea9c9228f00769ea9c44f) is a
minimal example of the Finger protocol implemented in Python 3.

## Server

It runs over TCP on port 79 and essentially consists of one request and one
response. When the client opens a socket to the server, it sends a plaintext
string (terminated with a CRLF aka `\r\n`) containing the name of the user to
query. If the user is found on the server's system, the system should return
any information about the user that the system administrator deems fit to send.
This could include last login time, number of unread emails, their telephone
number, or none of the above. Immediately following this information should be
the contents of the user's `.plan` file in their home directory. This file can
can be used as a form of microblogging, to-do lists, or simply note taking that
other people are allowed to view.

There is only one recognized option switch in the protocol. If the username is
prefixed with `/W` and a space, the server is expected to deliver more verbose
information about the requested user.

Naturally, this is a *very* flexible standard. The daemon running on port 79
could theoretically return anything, though it is expected to reutrn some kind
of plaintext document written by the requested user (presumably a `.plan`).

The original protocol used ASCII encoding by necessecity, but a modern
implementation could probably get away with using UTF-8.

It is also extremely paramount that both the server and client programs be made
secure from buffer overflow attacks. See the
[Morris worm](https://en.wikipedia.org/wiki/Morris_worm).

## Client

The client is also extremely simple. It is traditionally used from the command
line, with the name `finger`. The syntax for a `finger` command is as follows:

```sh
finger username@hostname
```

The client program is then expected to resolve `hostname` to a web address, and
then send `username` as the plaintext string to that address on port 79.

If you wish to get a more verbose answer from the server, the following syntax
should be used:

```sh
finger "/W username@hostname"
```
