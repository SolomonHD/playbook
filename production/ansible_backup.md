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
### 2. Prequsites for Script Running the script
The script requires Ansible 2.5.4, and the latest versions of boto, boto3, botocore, awscli. Prefered method of install is pip.
The script requires an aws access/secret key and also a region. There are different ways of setting these values but the prefered method is environment variables.
Create an .aws folder in the user's home directory. Create a file called credentials
The inside of this file should look like:
[backup\_profile\_name]
aws\_access\_key\_id = ACCESS\_KEY\_HERE
aws\_secret\_access\_key = SECRET\_KEY\_HERE

Now this profile can be set with the AWS\_PROFILE variable, also the region you want the script to run under will need to be set by AWS\_REGION variable.
export AWS\_PROFILE=backup\_profile\_name AWS\_REGION=us-east-1
