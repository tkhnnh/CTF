```
aws sts get-caller-identity
```
to get information about the user we have configured for the AWS CLI


Run `aws sts get-caller-identity`. What is the number shown for the "Account" parameter?
```
{
    "UserId": "g4daubicq6igkaxgrlc2",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/sir.carrotbane"
}
```

# IAM: Users, Roles, Groups and Policies
## IAM Overview
manage users and their access to various resources

### IAM Users
- Represents a single identity in AWS
- set of creds ( pass or access keys)
  -> Permissions can be granted at a user level defining the level of access a user might have

### IAM Groups
- Contains multiple users

### IAM Roles
- temporary identity assumed by a user, services or external accounts to get certain permissions

### IAM Policies
a JSON document defines the following:
- Action
- Resource
- Condition
- Principal

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificUserReadAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/Alice"
      },
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::my-private-bucket/*"
    }
  ]
}
```

What IAM component is used to describe the permissions to be assigned to a user or a group?
`policy`


## Enumerating Users
```
aws iam list-users
```

## Enumerating User Policies
```
aws iam list-user-policies --user-name {name}
```

Find attached policies
```
aws iam list-attached-user-policies --user-name {name}
```

Check if that user is part of a group
```
aws iam list-groups-for-user --user-name {name}
```

Let's see what permissions this policy grants by running the following command
```
aws iam get-user-policy --policy-name {POLICYNAME} --user-name {name}
```


What is the name of the policy assigned to sir.carrotbane?
```
{
    "PolicyNames": [
        "SirCarrotbanePolicy"
    ]
}
```

## Enumerating Roles
```
aws iam list-roles
```

 To check the inline policies
 ```
aws iam list-role-policies --role-name {name}
```

 let's see if there are any attached policies assigned to the role as well.
```
aws iam list-attached-role-policies --role-name {name}
```

Let's see what permissions we can get from the policy.
```
aws iam get-role-policy --role-name {name} --policy-name {POLICYNAME}
```
 
 Use AWS STS to obtain the temporary credentials that enable us to assume this role. 
```
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/bucketmaster --role-session-name TBFC
```
Apart from GetObject and ListBucket, what other action can be taken by assuming the bucketmaster role?
`ListAllMyBuckets`



# What is S3?
Simple Stroage Service
- store any type of object (images, documents, logs and backup files)
=> Any object store in S3 will be put into a "Bucket"

 list the available S3 buckets by running the following command:
```
aws s3api list-buckets
```

 Let's check out the contents of this directory.
```
aws s3api list-objects --bucket easter-secrets-123145
```

 let's copy the file in this directory to our local machine. This might have a secret message.
```
aws s3api get-object --bucket easter-secrets-123145 --key cloud_password.txt cloud_password.txt
```


What are the contents of the cloud_password.txt file?
THM{more_like_sir_cloudbane}

