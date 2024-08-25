```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 create-vpc  --cidr-block 10.1.0.0/16 --region us-east-1
```

```
{
    "Vpc": {
        "CidrBlock": "10.1.0.0/16",
        "DhcpOptionsId": "dopt-2cebea56",
        "State": "pending",
        "VpcId": "vpc-068cb58d49a0f77dd",
        "OwnerId": "044625059057",
        "InstanceTenancy": "default",
        "Ipv6CidrBlockAssociationSet": [],
        "CidrBlockAssociationSet": [
            {
                "AssociationId": "vpc-cidr-assoc-0da6986c3ddb07393",
                "CidrBlock": "10.1.0.0/16",
                "CidrBlockState": {
                    "State": "associated"
                }
            }
        ],
        "IsDefault": false
    }
}
```

```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 create-subnet --vpc-id vpc-068cb58d49a0f77dd --cidr-block 10.1.1.0/24 --region us-east-1
```
```
{
    "Subnet": {
        "AvailabilityZone": "us-east-1b",
        "AvailabilityZoneId": "use1-az6",
        "AvailableIpAddressCount": 251,
        "CidrBlock": "10.1.1.0/24",
        "DefaultForAz": false,
        "MapPublicIpOnLaunch": false,
        "State": "available",
        "SubnetId": "subnet-0288e5c6f3224ef59",
        "VpcId": "vpc-068cb58d49a0f77dd",
        "OwnerId": "044625059057",
        "AssignIpv6AddressOnCreation": false,
        "Ipv6CidrBlockAssociationSet": [],
        "SubnetArn": "arn:aws:ec2:us-east-1:044625059057:subnet/subnet-0288e5c6f3224ef59",
        "EnableDns64": false,
        "Ipv6Native": false,
        "PrivateDnsNameOptionsOnLaunch": {
            "HostnameType": "ip-name",
            "EnableResourceNameDnsARecord": false,
            "EnableResourceNameDnsAAAARecord": false
        }
    }
}
```

```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 create-internet-gateway --region us-east-1
```
```
{
    "InternetGateway": {
        "Attachments": [],
        "InternetGatewayId": "igw-083f12ad075917a3d",
        "OwnerId": "044625059057",
        "Tags": []
    }
}
```

```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 attach-internet-gateway --vpc-id vpc-068cb58d49a0f77dd --internet-gateway-id igw-083f12ad075917a3d --region us-east-1
```

```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 create-route-table --vpc-id vpc-068cb58d49a0f77dd --region us-east-1
```
```
{
    "RouteTable": {
        "Associations": [],
        "PropagatingVgws": [],
        "RouteTableId": "rtb-0487c76275487bdca",
        "Routes": [
            {
        "Routes": [
            {
                "DestinationCidrBlock": "10.1.0.0/16",
                "GatewayId": "local",
                "Origin": "CreateRouteTable",
                "State": "active"
            }
        ],
        "Tags": [],
        "VpcId": "vpc-068cb58d49a0f77dd",
        "OwnerId": "044625059057"
    },
    "ClientToken": "28fdb71e-5322-4498-8a53-b1892780d5e2"
}
```

```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 create-route --route-table-id rtb-0487c76275487bdca --destination-cidr-block 0.0.0.0/0 --gateway-id igw-083f12ad075917a3d --region us-east-1
```
```
{
    "Return": true
}
```

```
[cloudshell-user@ip-10-130-93-166 ~]$ aws ec2 associate-route-table  --subnet-id subnet-0288e5c6f3224ef59 --route-table-id rtb-0487c76275487bdca --region us-east-1
```
```
{
    "AssociationId": "rtbassoc-0ab2b402649ac51b4",
    "AssociationState": {
        "State": "associated"
    }
}
```
