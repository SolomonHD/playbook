# AWS AMI Backups Driven by Ansible
This document will  outline steps needed to run the new ansible AMI backup.

### 1. The Ansible script
The ansible script is located at https://github.com/curationexperts/ami-backup-cm
There are two aspects to the script:
1. Create AMIs
The script will search for EC2 tagged with a tag named "NightlyBackup" which has a value of Yes.
It will then stop these instances, create the AMI, then start the EC2s back up.
When the AMI are created they are tagged with a "AMICreation" tag which simply has the day the AMI's were created.
2. Delete Old AMIs
The 2nd phase of the script searches AMI that have a AMICreation tag value of 30 days in the past. 
It then deletes those AMI's and their associated snapshots.
### 2. Prequsites for Running the Script Locally
The script requires Ansible 2.5.4, and the latest versions of boto, boto3, botocore, awscli. Prefered method of install is pip.
The script requires an aws access/secret key and also a region. There are different ways of setting these values but the prefered method is environment variables.
Create an .aws folder in the user's home directory. Create a file called credentials
The inside of this file should look like:

[backup\_profile\_name]
aws\_access\_key\_id = ACCESS\_KEY\_HERE
aws\_secret\_access\_key = SECRET\_KEY\_HERE

Now this profile can be set with the AWS\_PROFILE variable, also the region you want the script to run under will need to be set by AWS\_REGION variable.

export AWS\_PROFILE=backup\_profile\_name AWS\_REGION=us-east-1

It is now possible to run the script.
### 3. Running the Script on the Dedicated Ansible Server
While the script can we ran locally for production it is better practice to run the script in a dedicated EC2. The EC2 can stay on 24/7 running the script whenever desired. This also creates a central place to look for logs relating to the script.

Currently the instance acting as the server is running Amazon Linux 2 and can be reached at *34.228.242.138* as the *ec2-user*, the private key for logging in can be found in dce-hosting S3 bucket under the path *dce-util/general\_key.pem*
