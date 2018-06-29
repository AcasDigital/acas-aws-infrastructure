# Background
Infrastructure as Code for Acas Advice Alerting, including SNS topics and subscriptions

# Cloudwatch Alarms/Events etc SNS alert topics

## Prerequisites

## Deployment

Ideally these would be created in an Acas Shared Services (aka Tooling) AWS
Account. Cross-account sharing would allow reuse across accounts. We don't
have that account structure yet. We only have a pre-prod (containing Dev
and UAT environments), and a prod account (containing the Prod environment).

So for now, we're creating them in each account.

**PLEASE NOTE**

> All of the following CLI examples assume that your local AWS CLI
> configuration has a profile named `acas-pre-prod` and `acas-prod`. If
> your profiles are named differently, you will need to adjust
> appropriately.

For both the `acas-prod` and `acas-pre-prod` accounts, you'll need to
do something like:

```
pushd path/to/this/directory
ACAS_APP=ops-alerts
ACAS_ENV=pre-prod
ACAS_ACCOUNT=THE-ACCOUNT-ID
```

ACAS_ENV should be one of `prod` or `pre-prod`.

Package up your change:

```sh
aws cloudformation package --template-file "$(pwd)/${ACAS_APP}.template" \
    --s3-bucket "acas-cfn-${ACAS_ACCOUNT}-eu-west-1" --s3-prefix "${ACAS_APP}/$ACAS_ENV" \
    --output-template-file "$(pwd)/${ACAS_APP}-${ACAS_ENV}.packaged" \
    --profile "acas-${ACAS_ENV}"
```

Deploy the stack. For production, you might want to use the flag
`--parameter-overrides` to set the email address subscribed to the alerts.

So add `--parameter-overrides pSnsEmailAddress=monitoring@acas.org.uk` or
similar.

```sh
aws cloudformation deploy --stack-name "ACAS-${ACAS_APP}-${ACAS_ENV}" \
    --template-file "$(pwd)/${ACAS_APP}-${ACAS_ENV}.packaged" \
    --s3-bucket "acas-cfn-${ACAS_ACCOUNT}-eu-west-1" --s3-prefix "${ACAS_APP}/$ACAS_ENV" \
    --no-execute-changeset \
    --capabilities CAPABILITY_NAMED_IAM \
    --profile "acas-${ACAS_ENV}"
```

Take note of the change set name that it returned as use that:

```sh
aws cloudformation describe-change-set \
    --change-set-name INSERT_CHANGE_SET_NAME
    --profile "acas-${ACAS_ENV}"
```

If you wish to proceed, then execute the change set.

```sh
aws cloudformation execute-change-set \
    --change-set-name INSERT_CHANGE_SET_NAME
    --profile "acas-${ACAS_ENV}"
```

Alternatively, delete the change set and try again.

```sh
aws cloudformation delete-change-set \
    --change-set-name INSERT_CHANGE_SET_NAME
    --profile "acas-${ACAS_ENV}"
```
