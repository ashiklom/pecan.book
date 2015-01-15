The following are Mike's rough notes from a first attempt to port the PEcAn VM to the AWS. This was done on a Mac

These notes are based on following the instructions [here](http://www.rittmanmead.com/2014/09/obiee-sampleapp-in-the-cloud-importing-virtualbox-machines-to-aws-ec2/)

### Set up an account on [AWS](http://aws.amazon.com/)

After you have an account you need to set up a user and save your [access key and secret key](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)

In my case I created a user named 'carya'

### Install [AWS Command Line Interface](http://aws.amazon.com/cli/)
`pip install awscli`

Then set your user credentials

Set your credentials as environment variables:

`
export AWS_ACCESS_KEY=xxxxxxxxxxxxxx
export AWS_SECRET_KEY=xxxxxxxxxxxxxxxxxxxxxx
`

### Create an AWS S3 'bucket' to upload VM to

Go to [https://console.aws.amazon.com/s3](https://console.aws.amazon.com/s3) and click "Create Bucket"

In my case I named the bucket 'pecan'

### Upload

`
time ec2-import-instance OBIEE-SampleApp-v406-disk1.vmdk --instance-type m3.large --format VMDK --architecture x86_64 --platform Linux --bucket rmc-vms --region eu-west-1 --owner-akid $AWS_ACCESS_KEY --owner-sak $AWS_SECRET_KEY

`
