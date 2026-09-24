# Validations CLI Commands 

I used AWS Cloud Shell to perform a series of commands to ensure my configuration were implemented. 

## 1. Confirmed my PSelvy account assumed the role as the ***Administrator*** with the appropriate privileges through the IAM Identity Center. 
'''bash
aws sts get-caller-identity --query Arn --output text
```
**Output**
```text
arn:aws:sts:xxxxxxxxx:assumed-role/AWSReservedSSO_AdministratorAccess_fa753c7bbf9c15ae/Pselvy
```
[x] `assumed-role/AWSReservedSSO_...` confirms temporary Identity Center credentials, not the root user.



## 2. Validates that I do have ***Root*** user, and it secured using a MFA as recommended. 
```bash
 aws iam get-account-summary \
 --query 'SummaryMap.{RootMFA:AccountMFAEnabled,RootAccessKeys:AccountAccessKeysPresent}' \
  --output table
```

**Output**
```text
-------------------------------
|      GetAccountSummary      |
+-----------------+-----------+
| RootAccessKeys  |  RootMFA  |
+-----------------+-----------+
|  0              |  1        |
+-----------------+-----------+
```

## 3. Identity Center users and groups 
```bash
$ aws identitystore list-users --identity-store-id "$STORE_ID" \
  --query 'Users[].UserName' --output table

$ aws identitystore list-groups --identity-store-id "$STORE_ID" \
 --query 'Groups[].DisplayName' --output table
```
**Output**
```text
--------------------
|     ListUsers    |
+------------------+
|  Pselvy          |
|  Support-tester  |
+------------------+

-------------------
|   ListGroups    |
+-----------------+
|  Admin          |
|  ReadonlyUsers  |
+-----------------+
```

## 4. Permission sets (e.g., AdministratorAccess)
```bash
$ for ps in $(aws sso-admin list-permission-sets --instance-arn "$INSTANCE_ARN" \
  --query 'PermissionSets[]' --output text); do
   aws sso-admin describe-permission-set --instance-arn "$INSTANCE_ARN" \
   --permission-set-arn "$ps" \
    --query 'PermissionSet.{Name:Name,Session:SessionDuration}' --output table
> done
```
**Output**

```text
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
```
