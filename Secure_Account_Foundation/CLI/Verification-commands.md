
Validations CLI Commands 

$ aws iam get-account-summary --query 'SummaryMap.AccountMFAEnabled'
1
$ 
$ # Trail exists and is logging?
$ aws cloudtrail describe-trails
{
    "trailList": [
        {
            "Name": "org-account-trail",
            "S3BucketName": "aws-cloudtrail-logs-153585581852-343b0fa8",
            "IncludeGlobalServiceEvents": true,
            "IsMultiRegionTrail": true,
            "HomeRegion": "us-east-1",
            "TrailARN": "arn:aws:cloudtrail:us-east-1:153585581852:trail/org-account-trail",
            "LogFileValidationEnabled": true,
            "HasCustomEventSelectors": true,
            "HasInsightSelectors": false,
            "IsOrganizationTrail": true,
            "RecursiveLogging": true
        }
    ]
}
$ aws cloudtrail get-trail-status --name org-account-trail --query 'IsLogging'
true

aws budgets describe-budgets --account-id 
{
    "Budgets": [
        {
            "BudgetName": "Monthly Budeget",
            "BudgetLimit": {
                "Amount": "10.0",
                "Unit": "USD"
            },
            "TimeUnit": "MONTHLY",
            "TimePeriod": {
                "Start": "2026-09-01T00:00:00+00:00",
                "End": "2087-06-15T00:00:00+00:00"
            },
            "CalculatedSpend": {
                "ActualSpend": {
                    "Amount": "0.0",
                    "Unit": "USD"
                },
            "BudgetLimit": {onthly Budeget",
{
    "Budgets": [
        {
            "BudgetName": "Monthly Budeget",
            "BudgetLimit": {
                "Amount": "10.0",
                "Unit": "USD"
            },
            "TimeUnit": "MONTHLY",
            "TimePeriod": {
                "Start": "2026-09-01T00:00:00+00:00",
                "End": "2087-06-15T00:00:00+00:00"
            },
            "CalculatedSpend": {
                "ActualSpend": {
                    "Amount": "0.0",
                    "Unit": "USD"
                },
:
