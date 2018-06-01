Infrastructure as code for Acas.

This is intended to define AWS resources and stacks for Acas.

The initial infrastructure was hand-cranked. As it gets bigger,
doing things by hand becomes more complicated. Using code to define it
means that we can iterate and refine ideas in a lower environment
(development) before safely promoting them to a higher environment
(production).

## Dependencies

You will need the AWS CLI tools, and appropriate configuration to allow
you to use the CLI to access the Acas AWS accounts.

