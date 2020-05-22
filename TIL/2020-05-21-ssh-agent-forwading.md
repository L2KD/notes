SSH agent forwarding

How

```
ssh -A host
```

Explained

https://smallstep.com/blog/ssh-agent-explained/

> SSH's agent forwarding feature allows your local SSH agent to reach through an existing SSH connection and transparently authenticate on a more distant server. For example, say you SSH into an EC2 instance, and you want to clone a private GitHub repository from there. Without agent forwarding, you'd have to store a copy of your GitHub private key on the EC2 host. With agent forwarding, the SSH client on EC2 can use the keys on your local computer to authenticate to GitHub.

**N.B: This comes with a risk**
