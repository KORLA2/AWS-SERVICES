## What is S3

- It is Simple , Popular, inexpensive storage service.

- To store your data in Amazon S3, you work with resources known as buckets and objects.

- One can store any file types such as docx, ppt, pdfs, txt files , videos etc.

- Not only these, other AWS services like

   - EC2: Stores snapshots and back ups in S3.

   - Cloud Watch : Exports Logs and metrics to S3.

   - Lambda : O/P's data logs to S3.

  Not only these there are many AWS services like RDS, Cloud Trail, Cloud Watch, Glue, Kinesis, EMR, RedShift, Athena etc use S3 for their data storage.

- S3 Provides unlimited data storage

- Objects stored in one Availability Zone is by default  replicated across multiple availability zones to ensure high availability .

## S3 Restrictions

• By default you can create 100 buckets/account

•No Maximum Bucket Size , No limit to the number of objects

•File Size can be 0-5TB

•Files larger than 100MB should use multi part upload.

## Bucket Naming Rules

•No Upper Cases, No Underscores , No Special characters   except  dot (.)  and hyphen (-)

•Name must be 3-63  characters long.

•Names can begin and end with number or character.

•Adjacent dots (.) or hyphens (-) are not allowed.

Bucket Name must be unique across all the AWS accounts across all the regions.

## Examples

•  cloud-newbie.1023  ✅ As this contains only . - numbers and small letters this name is allowed.

• abc..123 ❌ It Contains .. which violates Rule 4.

• ABC_123 ❌ It Contains _ and Capitals which violates Rule 1

• cloud-newbie  ✅ As this contains only . -

## Types of Buckets

•General Purpose Buckets

• Directory Buckets

## General Purpose Buckets

• Organizes data in a flat hierarchy , used for storing any type of data where no specific structure is required, like logs or backups.

•  This is original S3 Bucket type.

•Used with all storage classes except S3 Express One Zone.

•By default one can create 100 general buckets/account.

## Directory Buckets

•Organizes data in a folder hierarchy

•Only to be used with S3 Express One Zone.

•Recommended for single digit milli second performance on PUT,GET operations.

•100 directory buckets/account.

## S3 Directories

•S3 general purpose buckets does not have true folders found in the hierarchy .

•In Amazon S3, there is no such thing as a "real folder" even in "directory buckets.“

•When you create a folder, S3 creates  Zero Byte S3 Object with name that ends with /.

## Setting Auto Prompt

We have to set the below environment variable so that when we partially complete any aws commands CLI suggest remaining like VScode.

```
export AWS_CLI_AUTO_PROMPT=on-partial
```

<b>Playing with Buckets and Objects Using S3 and S3api Commands</b>

Explore all the commands using Auto Prompt.

## Create a bucket:

  mb=make bucket
```
 aws s3 mb s3://korla-goutham.1023
```

s3 command expects s3:// this is URI.

## Uploading object

  cp=copy
```
aws s3 cp Rocky.png s3://korla-goutham.1023
```
Now Rocky.png image will be uploaded to that korla-goutham.1023 bucket.

If you want to rename the object when it was uploaded to bucket
```
aws s3 cp Rocky.png s3://korla-goutham.1023/Narachi.png
```
Now Rocky.png will be renamed to Narachi.png in the bucket .

## List all the buckets and objects in the bucket

```
aws s3 ls
```

This Command lists all the buckets.

```
aws s3 ls s3://korla-goutham.1023
```
This Command lists all the objects inside the korla-goutham.1023 bucket.

## Download the object from bucket to local
```
aws s3 cp  s3://korla-goutham.1023/Rocky.png ~/Pictures
```
Now Rocky.png image will be downloaded to Pictures folder.

If I want to copy folder to bucket use recursive so that all the files will be copied to bucket.
```
aws s3 cp ~/Pictures s3://korla-goutham.1023 --recursive
```
## Remove Object from bucket

 rm=remove
```
aws s3 rm  s3://korla-goutham.1023/Rocky.png
```

This Command deletes Rocky.png image from the bucket.

If I want to delete all objects from the bucket.
```
aws s3 rm  s3://korla-goutham.1023 --recursive
```
## Delete Bucket

  rb=remove bucket
```
 aws s3 rb  s3://korla-goutham.1023
```
There is one more command called <b>s3api</b> which is low level and has more options than S3 command.

## 1. Create Bucket
```
aws s3api create-bucket  --bucket korla-goutham.1023
```
## 2. Upload Object to the bucket
```
aws s3api put-object --bucket korla-goutham.1023 --key Bhai.png  --body Rocky.png
```
—key=What name I want to put for the object while uploading to a bucket

—body=Local Object Name.

## 3. List Objects and Buckets
```
aws s3api list-objects --bucket korla-goutham.1023
```
This command lists all the objects in the bucket

```
aws s3api list-buckets
```
This Command lists all the buckets.

# Real-World Use Cases.

## 1. Get only few buckets/objects using CLI

Companies will be having 1000s of objects/buckets using —max-items we can limit the number of objects/buckets.
```
aws s3api list-objects --bucket korla-goutham.1023  --max-items 3
```
Now it displays only 3 objects from korla-goutham.1023.

## 2. Get only Object Names

The above command would display result in this way.

There is bunch of information from here if we some times only want to get specific fields just like object names in our case.

In this type of situations we have to use `—query` this is called jmes path which is used to fetch some fields from JSON.
```
aws s3api list-objects --bucket korla-goutham.1023   --query "Contents[].Key"
```
Contents is the array of objects , from this array we are fetching Key.

If you want to get multiple values the for example Key and last modified.

```
aws s3api list-objects --bucket korla-goutham.1023 --query "Contents[].{Key:Key,LastModified:LastModified}"
```

## 3. Filter objects with a specific prefix

Some times we might need to filter objects that was starting with some name .

For Example if we want to filter all the objects that was starting with name Movies
```
aws s3api list-objects --bucket korla-goutham.1023  --prefix Movies
```
`—prefix` is used to filter all the objects starting with name Movies.

For Example if we want to filter all the objects that contains name Movies in it
```
aws s3api list-objects --bucket korla-goutham.1023 --query "Contents[?contains(Key,'Movies')]"
```
`?contains(Key,’Movies’) function` checks each object name and verifies whether object name contains Movies in it or not.

## 4. Get the Largest Object in the bucket.
```
aws s3api list-objects --bucket korla-goutham.1023 --query "Contents|sort_by(@,&Size)[-1]"
```
`sort_by(@,Size) `function will sort objects according to the size in ascending order and -1 will fetch last one which means largest object.

If you want to get 3 largest objects
```
aws s3api list-objects --bucket korla-goutham.1023 --query "Contents|reverse(sort_by(@,&Size)[0:3])"
```
reverse function reverses the objects that are sorted in ascending order which means to descending order and [0:3] gives us the 1st 3 elements which means 3 largest objects.

## 5. List objects uploaded after a specific date?

This is how result looks like from this using LastModified field we can achieve this.

For Example If I want to filter all the objects that are uploaded after date:` 2024-12-11T18:44:15+00:00`
```
 aws s3api list-objects --bucket korla-goutham.1023 --query "Contents[?LastModified>2024-12-11T18:44:15+00:00]"
```
Now all the objects uploaded/modified after this date will be listed.

