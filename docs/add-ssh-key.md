# How to add SSH key to your Conga

Key based authentication in SSH is called public key authentication. SSH keys are an easy way to identify trusted computers, without involving passwords. The steps below will walk you through generating an SSH key and adding the public key to your Conga.

## Prerequisites

You need to be able to [access your Conga by SSH](./rooting-conga-3x90.md).

## Generate an SSH Key in your computer

We use OpenSSH in our computer. With OpenSSH, an SSH key is created using `ssh-keygen`.

```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ylo/.ssh/id_rsa): mykey
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in mykey.
Your public key has been saved in mykey.pub.
The key fingerprint is:
SHA256:GKW7yzA1J1qkr1Cr9MhUwAbHbF2NrIPEgZXeOUOz3Us ylo@klar
The key's randomart image is:
+---[RSA 2048]----+
|.*++ o.o.        |
|.+B + oo.        |
| +++ *+.         |
| .o.Oo.+E        |
|    ++B.S.       |
|   o * =.        |
|  + = o          |
| + = = .         |
|  + o o          |
+----[SHA256]-----+
$
```

The key pair (public key and private key) are usually stored in the `~/.ssh` directory.

## Copy the key to your Conga

Conga use Dropbear as SSH Server. We will need to copy key in the `/etc/dropbear` directory. From our computed:

```
$ ssh root@<conga ip> "tee -a /etc/dropbear/authorized_keys" < ~/.ssh/id_rsa.pub
```

This logs into the server host, and copies keys to the server, and configures them to grant access by adding them to the authorized_keys file. The copying will ask for a password.

## Test the new key

Once the key has been copied, it is best to test it:

```
$ ssh root@<conga ip>
```

The login should now complete without asking for a password. Note, however, that the command might ask for the passphrase you specified for the key.

## Sources

- [OpenWRT Dropbear Public Key](https://openwrt.org/docs/guide-user/security/dropbear.public-key.auth)
- [copy-id](https://www.ssh.com/ssh/copy-id)
