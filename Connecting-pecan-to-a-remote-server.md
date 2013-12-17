### If the remote machine supports ssh keys

Before the first time, run `scripts/sshkey.sh`

### If the remote machine does not support ssh keys

Add the following to `~/.ssh/config`

```
Host <hostname goes here>
 ControlMaster auto
 ControlPath /tmp/%r@%h:%p
```

Next create a single ssh connection to the host, any ssh connection
afterwards will use the same connection and should not require a password.

Before running the PEcAn workflow, open (and leave open) a single ssh connection from the local machine to the remote machine