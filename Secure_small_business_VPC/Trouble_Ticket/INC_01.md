# Public website down 
## Incident 01: 
*Desert Bloom Dental office manager reports that their customers are not able to reach their website. Desert Bloom has also provided the flowing screenshot for validation of problem*
![Website can't be reached](./Secure_small_business_VPC/screenshot/website_down.png)

### Step 1: Conformation
1. I confirmed the outage by going to *dbd-web* public IP 44.220.71.189, and was able to replicate the error. 
2. Ran *reach ability analysis* under VPC and the test **FAILED**
   ![Failed Test](./screenshot/reachability_ability_analyis_fail.png)
**LOG**"
```bash
Analysis explorer
Source
igw-052d754ac97ca3b96
Source account ID
153585581852
Destination
i-09cef567a473feb7a
Destination account ID
153585581852
Reachability status
Not reachable
Last analysis date
October 7, 2026, 16:34 (UTC-07:00)
Intermediate component filter
-
Exclude intermediate component
-
Destination is not reachable. For more information, see the explanations below.
Give us feedback
Explanations
**Route table rtb-0d8964850ad582d7f does not have an applicable route to igw-052d754ac97ca3b96. See documentation** 
Details
{
  "Destination": {
    "Id": "igw-052d754ac97ca3b96",
    "Arn": "arn:aws:ec2:us-east-1::internet-gateway/igw-052d754ac97ca3b96"
  },
**"ExplanationCode": "NO_ROUTE_TO_DESTINATION",**
  "RouteTable": {
    "Id": "rtb-0d8964850ad582d7f",
    "Arn": "arn:aws:ec2:us-east-1:1:route-table/rtb-0d8964850ad582d7f"
  },
  "Vpc": {
    "Id": "vpc-0fff4fd233db6aed7",
    "Arn": "arn:aws:ec2:us-east-1::vpc/vpc-0fff4fd233db6aed7"
  }
}
```
While reviewing the log I noticed there was a missing route for *dbd-igw* to accept request from internet users. 

3. I reviewed *dbd-public-rt* route table and noticed that 0.0.0.0/0 targeting *dbd-igw*
4. Added missing route back to *dbd-public-rt*
5. Confirmed my solution by running *reach ability analysis* and going to the website
![Passed Test](./screenshot/reachability_ability_analyis_pass.png)
![Web Page](./screenshot/DBD_Web_Page_Active.png)

Root Cause: route of last resort 0.0.0.0/0 targeting *dbd-igw* was missing from the *dbd-public-rt*.

[BACK](../README.md)


[BACK](../README.md)
