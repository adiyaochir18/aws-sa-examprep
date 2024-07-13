## Create a user with no permissions
New user with no permissions and generate out access keys
```sh
aws iam create-user --user-name sts-machine-test
aws iam create-access-key --user-name sts-machine-test --output table               // Creating a access key to the user
aws configure
```
Then edit credentials file to change the default profile
Testing who u are :

```sh
aws sts get-caller-identity --profile sts
aws s3 mb s3://adiya-exam-training            //Or any other command should raise an error sayign access denied
```


## Create a Role using cloudformation and give access for s3 specific bucket
```sh
chmod u+x bin/deploy
./bin/deploy        //to run
```

## Use new user creadentials and assume role

```sh
aws iam put-user-policy \
--user-name sts-machine-test \
--policy-name AssumePolicy \
--policy-document file://policy.json
```


```sh
aws sts assume-role \
--role-arn arn:aws:iam::992382440231:role/StsRole-StsRole-oX3NSpkMOXvN \
--role-session-name s3-access-sts \
--profile test-sts
```


## Assumed credentials 

"Credentials": {
        "AccessKeyId": "ASIA6ODU2KMTROAHIQ2A",
        "SessionToken": "IQoJb3JpZ2luX2VjEMv//////////wEaDGV1LWNlbnRyYWwtMSJIMEYCIQCFDssgHrKJspu5JPkrVoDyD+vmCCp4d3pRNYQzNzJAHAIhAMgQXQlebyyjbDYqrwPSrjPAtxZ5m6VTWn+p/i8Qd0D/KqMCCJT//////////wEQABoMOTkyMzgyNDQwMjMxIgyyVTy99euUdrgwR90q9wGBUGZHXsfYP6968FjV/DryYBDH5hQalWGpmJOqC1+802vbHkAe9a23EzvzG1IDzIml/SZHUvlWPxWjReIHhN0X+8xmxqhd2ojh/27dqg/zF/NBKgHg8nHnL2JbgHWQGQQPVcWMekMBbIRSVF7qMzfnTPdSvA/Wp89Oa2/HxTd1vxBnGezS7HaNHUCgTUqQDzNceU9/Xa46eTFllXBv7bG+ICPnxG8XCniGBqMTVHaUD//stBkBj1vyoU1b5avy2ibqYtGm0zQpRjUhV4lThnRdhkss/ZT0m+jPZb/k04oMOuby7QGOpwBUQXy66FV/2WnXn9bdCHD4I/Hceg2a7H1+/v64ZbwqhVD5UvDhf9lDuhZiA8+BPtzyP2J+Wcqgjh7B7dabjyfGakK2EIhjkyqrAZHAJCyRCy7FvGUit228IjNrB/0jjqzN7iLifGODwv4iLO3yelNEJDt29X0t7iS14xDfooz3Il6YH7nGqlLMX/QhDKWrN8lQBLWvsSONcfN1VcB",
        "Expiration": "2024-07-13T19:56:43+00:00"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROA6ODU2KMT4EY6K4FQK:s3-access-sts",
        "Arn": "arn:aws:sts::992382440231:assumed-role/StsRole-StsRole-oX3NSpkMOXvN/s3-access-sts"
    }
}

## Cleanup

Delete cloudformation stack via aws management console

```sh
aws iam delete-user-policy \
    --user-name sts-machine-test \
    --policy-name AssumePolicy

aws iam delete-access-key --access-key-id AKIA6ODU2KMT7YR3CMWA --user-name sts-machine-test

aws iam delete-user --user-name sts-machine-test
```