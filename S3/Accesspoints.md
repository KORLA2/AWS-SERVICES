
## Use of Access points

Lets consider a scenario today if you want to give read permissions to some objects for a user , you will use simply bucket policy to do that .

Tomorrow if you want to give some other permissions to a role you will again use the same bucket policy.

Other day when you want to give permissions to a group if you keep on using this bucket policy this will grow and it becomes hard to read .
 ![image](https://github.com/user-attachments/assets/4f25ccd4-2ab2-4c34-8a38-4a935531d6a8)


That’s where access points comes in rescue🦸 , instead of giving permissions to every one in one bucket policy you will create individual access points 
for each one and give permissions to these access points and all these access points are attached to the bucket.

![image](https://github.com/user-attachments/assets/bdd3cc05-c734-4c8b-b7f6-9eff580047bb)


In this way bucket policy wont grow .

Now its time to get our hands dirty .

![StickyHandsGIF (2)](https://github.com/user-attachments/assets/1f33ad9c-4e7a-42d5-939e-bfebb1915461)


Let’s consider a scenario where you need to allow access to the objects in your bucket only through access points , 
and also grant access to a specific user in another account . Sounds simple, right?"

## NAAM CHOTA HAI LEKIN SOUND BADA HAI

## 1. Firstly let’s create access point.
```
 aws s3control create-access-point --account-id 9848032919 --name my-access-point \
--bucket my-bucket
```

Now this will create access point its network origin is internet if you want to create VPC as network origin then No internet access. 
Requests are made over a specified VPC only. This will create access point and is attached to the bucket.

Grab the access point arn.

## 2.  Next let’s grant permissions to this access point

I will now grant user KGF to access the bucket and its objects its simple!!! only if you know already know how to write bucket policy.
That’s why have a quick glance at my above attached blog.

Access point policy is similar to bucket policy.

Lets grant KGF user, get object and list bucket permissions , in the access point policy Resource is arn of the access point .
```
// access-policy.json

{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": { "AWS": "arn:aws:iam::9848032919:user/KGF" },
        "Action": [
          "s3:GetObject",
          "s3:ListBucket" 
        ],
        "Resource": [
          "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point",
          "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point/object/*"
        ]
      }
    ]
  }
```
Now KGF user will be having get object and list bucket permission on the bucket to which this access point was attached.

## 3. Attach the permissions to access point

Now attach this access point policy to the access point.
```
aws s3control put-access-point-policy --account-id 9848032919 --name my-access-point \
 --policy file://access-policy.json
```
Now if KGF user tries to access the bucket using …
```
aws s3 ls my-bucket
```
You will get error as KGF is fresh user this user doesn’t have any IAM permissions to access the object.

![image](https://github.com/user-attachments/assets/b0f99bde-2c42-4b0f-b314-0faba5c854df)


If KGF user has IAM permissions then this user will be able to . But we are interested in accessing the objects in the bucket using access point .

![image](https://github.com/user-attachments/assets/899de710-b7ff-4585-8397-a7d13fb4d31f)

Now also we got same error .

## 4. Access Point-Based Bucket Permissions

Now we have to configure bucket policy such that this will receive permissions from access point policy on the bucket/objects.
```
// bucket-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ],
      "Condition": {
        "StringEquals": {
          "s3:DataAccessPointArn": "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point"
        }
      }
    }
  ]
}
```
There is one condition called DataAccessPointArn which identifies that access is coming from which accesspoint.

Now this policy allows permissions from accesspoint.

Uploading this bucket policy ..
```
 aws s3api put-bucket-policy --bucket my-bucket --policy file://bucket-policy.json
```
Now If I try to access the objects again as a KGF user …
![image](https://github.com/user-attachments/assets/1d96f85e-43aa-4e0a-8e65-66be97b6a073)


I got the result .😁😁😁.

Now that we have allowed KGF user using access point , now our mini goal is to allow users to access the bucket/objects only via access point.

## 5. Allowing traffic only through access point.

As you guessed we have to change bucket policy to do that.
```
//bucket-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "s3:DataAccessPointArn": "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point"
        }
      }
    },
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:*",
       "Resource": [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ],
      "Condition": {
        "StringEquals": {
          "s3:DataAccessPointArn": "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point"
        }
      }
    }
  ]
}
```
Don’t get panic I am here to explain this.

1st rule is to deny every traffic unless it is made through access point .

2nd rule is to allow traffic from access point .

Seems Confusing right ? Deny rules has more precedence than allow rules, so deny rule here in bucket policy ,
denies every one even access point permissions and over writes the access point policy , and we are again allowing permissions to flow from access point .

Uploading this bucket policy ..
```
 aws s3api put-bucket-policy --bucket my-bucket --policy file://bucket-policy.json
```
When I access as KGF user …
![image](https://github.com/user-attachments/assets/f7855535-8087-456b-b6cb-5d44756d74b7)


This worked ..

OK but lets try accessing the objects in the bucket using normal S3 URI instead of access point .
![image](https://github.com/user-attachments/assets/34490cc6-3946-4669-a6d8-8be1f0c179f9)


I got explicit deny error and that’s worked hurray!!!

## 6.Cross Account Access

Now we have almost completed our requirement, next step is we have to allow user from the other account to access objects in the bucket in our account .

If you give arn of the user in the other account as a Principal in the access point policy, only this is not enough to do so. The User in the other account must have IAM permissions to access the s3 bucket as well as this access point in our account..

IAM policy to access S3 in other account :
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::my-bucket",
                "arn:aws:s3:::my-bucket/*"
            ]
        }
    ]
}
```
As the bucket names are unique that’s why no Account IDs in resource .

IAM policy to access the access point
```
 {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": [
				"s3:*"
			],
			 "Resource": [
              "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point",
              "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point/object/*"    
            ]
		}
	]
}
```
Now if the user has these 2 IAM Policies ,then after adding user arn as the Principal in the access point policy.

I have created user named Ali in the other account and also Ali has 2 IAM policies set.
```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": { "AWS": 
                     ["arn:aws:iam::9848032919:user/KGF",
                       "arn:aws:iam::9848022338:user/Ali" ] 
                         },
        "Action": [
          "s3:GetObject",
          "s3:ListBucket" 
        ],
        "Resource": [
          "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point",
          "arn:aws:s3:ap-south-1:9848032919:accesspoint/my-access-point/object/*"
        ]
      }
    ]
  }
```
When I try to access the bucket as Ali user…
![image](https://github.com/user-attachments/assets/65d118f0-04f6-48e1-bc95-4586202d494c)


I named Ali user as Goutham-Ali on my CLI . This works!!!

OK If I try to access using Normal S3 URI
![image](https://github.com/user-attachments/assets/991d8129-82f7-443b-95ec-0e3958749523)


This will not work.. 
