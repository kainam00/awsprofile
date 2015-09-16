# awsprofile
Simple ruby class for loading local aws cli profiles for use with the ruby SDK.

This allows you to easily use your existing AWS CLI configuration from inside ruby scripts, which makes testing and running applications locally much easier.

"Profile" refers to what you pass in the --profile arguement when you run the aws cli. Configurable via aws configure --profile profilename

Passing the string "role" to the class consutructor can be done if using roles, as is often the case with EC2 machines.

Example:
```
@profile = "nameofmyprofile" # This is what you pass in --profile to aws cli
# Figure out credentials
credentials = Awsprofile.new(@profile)
if @profile == "role"
	# Don't pass credentials, we're using roles given to this box
	ec2 = Aws::EC2::Client.new(
		region: credentials.config["region"],
	)
else
	ec2 = Aws::EC2::Client.new(
		# Pass credentials
	  region: credentials.config["region"],
	  credentials: Aws::Credentials.new(credentials.config["aws_access_key_id"], credentials.config["aws_secret_access_key"]),
	)
end
```
