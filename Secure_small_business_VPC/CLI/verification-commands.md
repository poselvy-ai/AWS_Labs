# AWS CLI Verification 

## Route Table

The following CLI verifies that the route tables are established between internet, region, and az.

``bash
~ $ aws ec2 describe-route-tables \

  --filters Name=vpc-id,Values=vpc-0fff4fd233db6aed7 \
  --query 'RouteTables[].{ID:RouteTableId,Name:Tags[?Key==Name]|[0].Value,Main:Associations[?Main==true]|[0].Main,Subnets:Associations[].SubnetId,Routes:Routes[].[DestinationCidrBlock,GatewayId,NatGatewayId]}' \
  --output json

[
    {
        "ID": "rtb-0afe9fd2451a6f748",
        "Name": null,
        "Main": null,
        "Subnets": [],
        "Routes": [
            [
                "10.0.0.0/16",
                "local",
                null
            ],
            [
                "0.0.0.0/0",
                "igw-052d754ac97ca3b96",
                null
            ]
        ]
    },
    {
        "ID": "rtb-0343a35f325e30fba",
        "Name": null,
        "Main": true,
        "Subnets": [],
        "Routes": [
            [
                "10.0.0.0/16",
                "local",
                null
            ]
        ]
    },
    {
        "ID": "rtb-03b9fa7b25929745e",
        "Name": "dbd-private-rt",
        "Main": null,
        "Subnets": [
            "subnet-05d4b573c91fe3e1d",
            "subnet-0ebe4f16f820f4df9"
        ],
        "Routes": [
            [
                "10.0.0.0/16",
                "local",
                null
            ],
            [
                "0.0.0.0/0",
                null,
                "nat-138ac0fe973d9ae01"
            ]
        ]
    },
    {
        "ID": "rtb-0d8964850ad582d7f",
        "Name": "dbd-public-rt",
        "Main": null,
        "Subnets": [
            "subnet-0dbc876c9a85b2281",
            "subnet-0b03fd06e4826d1c2"
        ],
        "Routes": [
            [
                "10.0.0.0/16",
                "local",
                null
            ],
            [
                "0.0.0.0/0",
                "igw-052d754ac97ca3b96",
                null
            ]
        ]
    }
]
(END)
```

## Project Verification

The following CLI commands verifies that the projects, subnets, route tables, and instances are sucessfully up and running before tear down.

```bash
~ $ export AWS_PAGER=""
~ $ VPC=vpc-0fff4fd233db6aed7
~ $ aws ec2 describe-subnets --filters Name=vpc-id,Values=$VPC --query 'Subnets[].{Name:Tags[?Key==`Name`]|[0].Value,CIDR:CidrBlock,AZ:AvailabilityZone,AutoPublicIP:MapPublicIpOnLaunch}' --output table
-----------------------------------------------------------------
|                        DescribeSubnets                        |
+-----------+----------------+----------------+-----------------+
|    AZ     | AutoPublicIP   |     CIDR       |      Name       |
+-----------+----------------+----------------+-----------------+
|us-east-1b |  True          |  10.0.20.0/24  |  dbd-public-b   |
|us-east-1b |  False         |  10.0.12.0/24  |  dbd-private-b  |
|us-east-1a |  True          |  10.0.1.0/24   |  dbd-public-a   |
|us-east-1a |  False         |  10.0.11.0/24  |  dbd-private-a  |
+-----------+----------------+----------------+-----------------+
~ $ aws ec2 describe-route-tables --filters Name=vpc-id,Values=$VPC --query 'RouteTables[].{Name:Tags[?Key==`Name`]|[0].Value,Subnets:join(`,`,Associations[].SubnetId||`[]`),Routes:join(` | `,Routes[].join(`>`,[DestinationCidrBlock,GatewayId||NatGatewayId||`-`]))}' --output table
-------------------------------------------------------------------------------------------------------------------------------
|                                                     DescribeRouteTables                                                     |
+----------------+------------------------------------------------------+-----------------------------------------------------+
|      Name      |                       Routes                         |                       Subnets                       |
+----------------+------------------------------------------------------+-----------------------------------------------------+
|  dbd-nat-rt    |  10.0.0.0/16>local| 0.0.0.0/0>igw-052d754ac97ca3b96  |                                                     |
|  None          |  10.0.0.0/16>local                                   |                                                     |
|  dbd-private-rt|  10.0.0.0/16>local| 0.0.0.0/0>nat-138ac0fe973d9ae01  |  subnet-05d4b573c91fe3e1d,subnet-0ebe4f16f820f4df9  |
|  dbd-public-rt |  10.0.0.0/16>local| 0.0.0.0/0>igw-052d754ac97ca3b96  |  subnet-0dbc876c9a85b2281,subnet-0b03fd06e4826d1c2  |
+----------------+------------------------------------------------------+-----------------------------------------------------+
~ $ aws ec2 describe-security-groups --filters Name=vpc-id,Values=$VPC --query 'SecurityGroups[].{Name:GroupName,Inbound:IpPermissions}' --output json
[
    {
        "Name": "dbd-app-sg",
        "Inbound": [
            {
                "IpProtocol": "tcp",
                "FromPort": 80,
                "ToPort": 80,
                "UserIdGroupPairs": [
                    {
                        "UserId": "153585581852",
                        "GroupId": "sg-09ba64186af98a186"
                    }
                ],
                "IpRanges": [],
                "Ipv6Ranges": [],
                "PrefixListIds": []
            }
        ]
    },
    {
        "Name": "default",
        "Inbound": [
            {
                "IpProtocol": "-1",
                "UserIdGroupPairs": [
                    {
                        "UserId": "153585581852",
                        "GroupId": "sg-0c6964b26845fede1"
                    }
                ],
                "IpRanges": [],
                "Ipv6Ranges": [],
                "PrefixListIds": []
            }
        ]
    },
    {
        "Name": "dbd-web-sg",
        "Inbound": [
            {
                "IpProtocol": "tcp",
                "FromPort": 80,
                "ToPort": 80,
                "UserIdGroupPairs": [],
                "IpRanges": [
                    {
                        "CidrIp": "0.0.0.0/0"
                    }
                ],
                "Ipv6Ranges": [],
                "PrefixListIds": []
            }
        ]
    }
]
~ $ aws ec2 describe-instances --filters Name=vpc-id,Values=$VPC Name=instance-state-name,Values=running --query 'Reservations[].Instances[].{Name:Tags[?Key==`Name`]|[0].Value,PrivateIP:PrivateIpAddress,PublicIP:PublicIpAddress,Subnet:SubnetId}' --output table
-------------------------------------------------------------------------
|                           DescribeInstances                           |
+---------+--------------+----------------+-----------------------------+
|  Name   |  PrivateIP   |   PublicIP     |           Subnet            |
+---------+--------------+----------------+-----------------------------+
|  dbd-app|  10.0.20.106 |  13.220.210.88 |  subnet-0dbc876c9a85b2281   |
|  dbd-web|  10.0.1.106  |  44.220.71.189 |  subnet-0b03fd06e4826d1c2   |
+---------+--------------+----------------+-----------------------------+
~ $ aws ssm describe-instance-information --query 'InstanceInformationList[].{Id:InstanceId,Ping:PingStatus}' --output table
-----------------------------------
|   DescribeInstanceInformation   |
+----------------------+----------+
|          Id          |  Ping    |
+----------------------+----------+
|  i-09cef567a473feb7a |  Online  |
|  i-071007985ad93f4aa |  Online  |
+----------------------+----------+
```
