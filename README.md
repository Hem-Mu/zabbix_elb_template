# zabbix_elb_template
6.0+

# Zabbix ELB Template
from https://gitlab.com/dxrpl/zabbix-elb-template
Many thanks for basic concepts.

# CAUTION
Do not use {ITEM.KEY} macro in any action operations. If you use it e.g. in notification, then entire key with resolved variables will be sent to your zabbix users as an alert message so they will get your AWS credentials for zabbix user.

# AWS config

Create new IAM policy and paste code below to json section:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "cloudwatch:GetMetricStatistics",
            "Resource": "*"
        }
    ]
}
```

Policy name is irrelevant but it's good practise to name it like zabbix_cloudwatch or so.

Create new user and attach it to newly created policy. And again it's recommended to name it zabbix or so.

AWS Access Key and AWS Secret Key enter into host macro as described in Template config section.



# Template config

Place the script elb_stats.py, target_elb_stats.py at zabbix-server externalscripts folder(default path is /usr/lib/zabbix/externalscripts), make sure it has execute permission(chmod +x).
Import the template elb_template.xml to Zabbix.
Create new host and attach the template to it.
Set following in host's macro section:


ELB name : {$INSTANCE_NAME}. 
Be aware that ELB instance name is not a ARN, DNS name. 
It looks like: app/_LB_NAME/123abc456

TargetGroup name : {$TARGET_GROUP}.
It looks like: targetgroup/_TG_NAME/123abc456

AWS credentials {$AWS_ACCESS_KEY} and {$AWS_SECRET_KEY}

AWS region : {$REGION}
