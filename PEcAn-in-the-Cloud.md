The following are Mike's rough notes from a first attempt to port the PEcAn VM to the AWS. This was done on a Mac

These notes are based on following the instructions [here](http://www.rittmanmead.com/2014/09/obiee-sampleapp-in-the-cloud-importing-virtualbox-machines-to-aws-ec2/)


### Convert PEcAn VM

AWS allows upload of files as VMDK but the default PEcAn VM is in OVA format

1. If you haven't done so already, download the [PEcAn VM](http://isda.ncsa.illinois.edu/download/index.php?project=PEcAn&sort=category)

2. Split the OVA file into OVF and VMDK files

`
tar xf <ovafile>
` 

### Set up an account on [AWS](http://aws.amazon.com/)

After you have an account you need to set up a user and save your [access key and secret key](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)

In my case I created a user named 'carya'

Note: the key that ended up working had to be made at [https://console.aws.amazon.com/iam/home#security_credential](https://console.aws.amazon.com/iam/home#security_credential), not the link above.

### Install [AWS Command Line Interface](http://aws.amazon.com/cli/) and EC2 command line tools
`pip install awscli`

[EC2 tools](http://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/set-up-ec2-cli-linux.html)

`wget http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip`

`sudo mkdir /usr/local/ec2`

`sudo unzip ec2-api-tools.zip -d /usr/local/ec2`

If need be, download and install [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

`export JAVA_HOME=$(/usr/libexec/java_home)`

`export EC2_HOME=/usr/local/ec2/ec2-api-tools-<version>`

`export PATH=$PATH:$EC2_HOME/bin`


Then set your user credentials as environment variables:

`export AWS_ACCESS_KEY=xxxxxxxxxxxxxx`

`export AWS_SECRET_KEY=xxxxxxxxxxxxxxxxxxxxxx`

### Create an AWS S3 'bucket' to upload VM to

Go to [https://console.aws.amazon.com/s3](https://console.aws.amazon.com/s3) and click "Create Bucket"

In my case I named the bucket 'pecan'


### Upload

In the code below, make sure to change the PEcAn version, the name of the bucket, and the name of the region. Make sure that the architecture matches the version of PEcAn you downloaded (i386 for 32 bit, x86_64 for 64 bit).

Also, you may want to choose a considerably larger instance type. The one chosen below is that corresponding to the AWS Free Tier

`
ec2-import-instance PEcAn32bit_1.2.6-disk1.vmdk --instance-type t2.micro --format VMDK --architecture i386 --platform Linux --bucket pecan --region us-east-1 --owner-akid $AWS_ACCESS_KEY --owner-sak $AWS_SECRET_KEY
`
Make sure to note the ID of the image since you'll need it to check the VM status.  Once the image is uploaded it will take a while (typically about an hour) for Amazon to convert the image to one it can run. You can check on this progress by running

`
ec2-describe-conversion-tasks <image.ID>
`

### Booting the VM

On the EC2 management webpage you should be able to see your new PEcAn image as an option under Launch Instance.

Before launching, you will want to update the firewall to open up additional ports that PEcAn needs -- specifically port 80 for the webpage and 8787 for RStudio. Port 22 (ssh/sftp) should be open by default.  Under "Security Groups" select "Inbound" then "Edit" and then add “Custom TCP Rule”.



