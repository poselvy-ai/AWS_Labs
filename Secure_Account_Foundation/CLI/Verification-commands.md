# Validations CLI Commands 

I used AWS Cloud Shell to perform a series of commands to ensure my configuration were implemented. 

# Confirmed my PSelvy account assumed the role as the ***Administrator*** with the appropriate privileges through the IAM Identity Center. 
'''bash
aws sts get-caller-identity --query Arn --output text
arn:aws:sts:xxxxxxxxx:assumed-role/AWSReservedSSO_AdministratorAccess_fa753c7bbf9c15ae/Pselvy


# Validates that I do have ***Root*** account, and it secured using a MFA as recommended. 
'''bash
 aws iam get-account-summary \
>   --query 'SummaryMap.{RootMFA:AccountMFAEnabled,RootAccessKeys:AccountAccessKeysPresent}' \
>   --output table
-------------------------------
|      GetAccountSummary      |
+-----------------+-----------+
| RootAccessKeys  |  RootMFA  |
+-----------------+-----------+
|  0              |  1        |
+-----------------+-----------+


# Identity Center users and groups 
'''bash
$ aws identitystore list-users --identity-store-id "$STORE_ID" \
>   --query 'Users[].UserName' --output table
--------------------
|     ListUsers    |
+------------------+
|  Pselvy          |
|  Support-tester  |
+------------------+

$ aws identitystore list-groups --identity-store-id "$STORE_ID" \
>   --query 'Groups[].DisplayName' --output table

-------------------
|   ListGroups    |
+-----------------+
|  Admin          |
|  ReadonlyUsers  |
+-----------------+


# Permission sets (e.g., AdministratorAccess)
'''bash
$ for ps in $(aws sso-admin list-permission-sets --instance-arn "$INSTANCE_ARN" \
>   --query 'PermissionSets[]' --output text); do
>   aws sso-admin describe-permission-set --instance-arn "$INSTANCE_ARN" \
>     --permission-set-arn "$ps" \
>     --query 'PermissionSet.{Name:Name,Session:SessionDuration}' --output table
> done
-------------------------------
|    DescribePermissionSet    |
+-----------------+-----------+
|      Name       |  Session  |
+-----------------+-----------+
|  ReadOnlyAccess |  PT1H     |
+-----------------+-----------+
------------------------------------
|       DescribePermissionSet      |
+----------------------+-----------+
|         Name         |  Session  |
+----------------------+-----------+
|  AdministratorAccess |  PT1H     |
+----------------------+-----------+
