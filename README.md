# Fully automated mining

![alt text](https://cdn-images-1.medium.com/max/800/1*jk1iOT_yh-SqD1JUlMWc1g.png)

## 1. Packer
Use Packer to build your own AMI for mining.

```
cp variables.example.json variables.json
# Edit variables.json with your information
packer build -var-file filename.json eth-rig-packer.json
```

## 2. Cloud Formation

The yml template is easier to edit, but CloudFormation doesn't support aliases, so you'll want to
turn the yml back into json before you upload if you edit it.

If you want to use my AMI's built from this packer file, just use the CFN template as written. You'll
want to register with [Mining Pool Hub](https://ethereum.miningpoolhub.com) so you have a username to
put in the parameters when you launch it.

From the AWS console, head over to CloudFormation, create a new stack and choose upload to S3 with the
`eth-mining-cfn.template` file.

If you want to be really agressive, in CloudFormation use a StackSet to launch the stack in every
region. You'll need to prepare you account to let StackSet control it, full details
[Here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs.html), but
the gist is you'll allow StackSet in your "administrator" account to assume other roles. Then you'll
Create a role in your "target" account that can be assumed from the "administrator" and has permissions
to create the resources needed by your stack. I'm using a single account for "administrator" and "target".

That's a pain, so I created a CFN template for it, just create a stack from
`AWSCloudFormationStackSetSingleAccount.yml` in this repo to create the required roles with a minimal policy.

Once that's all in place upload the template again, enter your AWS account number, and choose all four
US Regions. 

## 3. Profit!

At the time of writing, ETH was just over $30. I ran 8 GPUs at max $.25 per for 8 hours and earned...
0.00152448 ETH or about a buck fifty. Not so much profit, but I did learn quite a bit. I'll launch again
at a max around $0.04/GPU/hr, that way I'll only need ETH to double to make a profit.

## References
* [Have Fun by Mining Ether with AWS Spot](https://medium.com/@james.s.wiggins/get-rich-quick-by-mining-ether-with-aws-spot-c7b7a3bdc149)
* [Ethereum AWS Ubuntu Miner Node Installation and Setup](http://clusterfrak.com/fun/eth_miner/)
* [StackSet Prerequisites](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs.html)


## Donations

* Etherium: 0x93ea57f8fA6F70394E84f4A9f746A17efe8a4AAD
