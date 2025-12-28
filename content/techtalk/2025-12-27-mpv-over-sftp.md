---
title: "MPV over SFTP"
date: 2025-12-27
---

MPV uses FFmpeg to play media over sftp.  The format is sftp url.

```
mpv sftp://user@remote:port/path/to/file.mp4
```

Using the sftp url as argument instead of piping the file content
to MPV allows you to seek properly.  And it works in most cases.
However, it bugs out if you need to use a jump host (see `man ssh`).
Then mpv will start the player and maybe even play a second of the
video, but then it will freeze.  The issue here is not mpv, since
using ffplay directly causes the same issue.

To circumvent the issue, we need to setup an ssh tunnel with the
jump host, so that we can use localhost in sftp url instead of
the remote host.  Then, mpv won't have to deal with jump hosts.
It will simply read the file over the localhost connection,
and ssh/sftp will take care of forwarding the requests to the
appropriate port (22) of the appropriate host (remote).

```
ssh -NL 2343:remote:22 user@jumphost                # Create ssh tunnel
mpv sftp://user@localhost:2343/path/to/file.mp4     # Play through the tunnel
```
