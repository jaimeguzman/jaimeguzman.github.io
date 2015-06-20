Fix "Could not open a connection to your authentication agent." when using ssh-add {.blog-post-title}
----------------------------------------------------------------------------------

If you're trying to add identities to the authentication agent
using ssh-add you may get this error:

    Could not open a connection to your authentication agent.

The reason as the error message suggests is, ssh-add don't know how to
talk with the authentication agent.

The problem can be solved by setting SSH\_AUTH\_SOCK environment
variable.

If you run ssh-agent you should get some output like this:

    SSH_AUTH_SOCK=/tmp/ssh-agVZL13989/agent.13989; export SSH_AUTH_SOCK;
    SSH_AGENT_PID=13990; export SSH_AGENT_PID;
    echo Agent pid 13990;

now if you evaluate that command output in your shell, the variable will
be set:

    eval $(ssh-agent)

Source: [https://coderwall.com/p/rdi\_wq](https://coderwall.com/p/rdi_wq)

 

