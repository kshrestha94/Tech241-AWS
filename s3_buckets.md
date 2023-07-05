# s3 buckets - store content on the cloud simular to blob storage on azure
```
analogy "throwing unsorted data onto something" 

use AWS CLI using pycharm and upload python scripts 
```

## open pycharm 
```
- create s3_buckets folder directory 
- cd s3_buckets
- pip install awscli
```
# commands start with "aws"

```
aws configure
```
``` 
aws access key id: <private key ID Do not share>
```
```
aws secret access key: <secret access key Do not share>
```
```
defaut region name - eu-west-1
```
```
default output format: json
```
```
aws s3 ls (check s3 folder)
```

# create a bucket 
```
aws s3 mb s3://tech241-kevin-bucket --region eu-west-1
```
```
aws s3 ls (check buckets created )
```
# upload a file 

`create a .txt file`
```
aws s3 cp testfile.txt s3://tech241-kevin-bucket
```
```
aws s3 sync s3://tech241-kevin-bucket s3_download
```

`download`
``` 
aws s3 sync s3://tech241-kevin-bucket s3_download
```

`delete`
```
aws s3 rm s3://tech241-kevin-bucket/testfile.txt
```
`remove bucket`
```
aws s3 rb s3://tech241-kevin-bucket
```

`creating s3_bucket on pycharm` 

# import boto3 library
import boto3 # importing specific library

# set up a s3 connection
s3 =boto3.client("s3") #set connection and assign to variable

# create a bucket in the eu-west-1 region - just change name of bucket 
bucket_name = s3.create_bucket(Bucket= "tech241-kevin-python-bucket", CreateBucketConfiguration={"LocationConstraint":"eu-west-1"} )

# print the bucket name to confirm working script
print(bucket_name)

`uploading s3_bucket on pycharm`

# import library
import boto3

# connect to client
s3 = boto3.client("s3")

#upload text file as uploaded file and print 
bucket_content = s3.upload_file("testfile.txt", "tech241-kevin-python-bucket", "uploadedfile.txt")
print(bucket_content)

`download and read s3_bucket on pycharm`

# import boto3
import boto3

# connect to client
s3 = boto3.client("s3")

# download and create readfile.txt

download_file = s3.download_file("tech241-kevin-python-bucket", "uploadedfile.txt", "readfile.txt" )

print(s3.download_file)

`delete uploaded file from bucket s3_bucket in pycharm`

import boto3

# connect to client
s3 = boto3.client("s3")

# delete uploaded file

s3.delete_object(Bucket='tech241-kevin-python-bucket', Key='uploadedfile.txt')

`delete bucket s3_bucket`

import boto3

s3 = boto3.resource("s3")

# delete bucket command
bucket = s3.Bucket("tech241-kevin-python-bucket")
response = bucket.delete()

#print responce
print(response)
