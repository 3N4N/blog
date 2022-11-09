---
title: "SSH Keys On a Shared Machine"
date: 2022-11-09
---


Services such as GitHub often requires an ssh connection. The user, say
Bob, has to [generate a key-pair][1] to authenticate himself when
communicating with the service. The generated key-pair exists in Bob’s
machine in plaintext format.

The issue arises when Bob’s machine is shared among multiple users, some
or all of whom has root access: they can access Bob’s private key, which
will enable them to identify themselves as Bob and give them access to
all services Bob has access to.


## Encrypting Private Key

This problem can be circumvented by encrypting the private key:

```
# replace `id_ed25519` with your private key file
ssh-keygen -p -f ~/.ssh/id_ed25519
```

The command above will let Bob add a passphrase (or change it if there
is already an old passphrase) which will be required to decrypt the
private file. So even if other users can access the file, they cannot
use it without the passphrase.


## Forwarding SSH Connection

If the shared machine is accessed via ssh, Bob can forward the
connection from an authenticating agent, e.g., `ssh-agent`. It will use
the key-pair stored in Bob’s local machine (as opposed to the shared
remote machine) for authentication. So Bob will not have to generate a
key-pair in the remote machine. He just needs to add `-A` flag to the
ssh command below and the key-pair in his local machine will be used.

```
ssh -A bob@remote-machine
```

Be warned, though. The Linux manual (`man 1 ssh`) has this is to say:

> Agent forwarding should be enabled with caution. Users with the
> ability to bypass file permissions on the remote host (for the agent's
> UNIX-domain socket) can access the local agent through the forwarded
> connection. An attacker cannot obtain key material from the agent,
> however they can perform operations on the keys that enable them to
> authenticate using the identities loaded into the agent. A safer
> alternative may be to use a jump host (see -J).


## Testing the Connection

Bob can test if he can authenticate properly by using the command below.

```
ssh -T git@github.com
# Hi Bob! You've successfully authenticated,
# but GitHub does not provide shell access.
```

[1]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
