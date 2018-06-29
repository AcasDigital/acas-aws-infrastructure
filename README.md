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

## Tricks

### AWS Account ID

For a lot of these commands, we want to get the AWS account ID and use
that to namespace some of the resources we create.

Assuming you've configured your AWS CLI tool with a profile named
`acas-pre-prod`, you can execute this command to get the AWS Account ID:

```sh
aws sts get-caller-identity --output text --query 'Account' --profile acas-pre-prod
```
