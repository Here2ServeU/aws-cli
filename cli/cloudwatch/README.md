## Step 1: Create Log Group
aws logs create-log-group --log-group-name t2s-web-server-log-group

## Step 2: Create Log Stream
aws logs create-log-stream --log-group-name t2s-web-server-log-group --log-stream-name t2s-web-server-log-stream

## Step 3: Creating Metrics

### Set CPU Metric
aws cloudwatch put-metric-alarm --alarm-name "HighCPUUtilization"
--metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300
--threshold 80 --comparison-operator GreaterThanThreshold
--dimensions Name=InstanceId,Value=i-0c1d24c351039f6e1 --evaluation-periods 1
--alarm-actions arn:aws:sns:us-east-1:730335276920:alarm-topic --unit Percent

### Set Memory Metric
aws cloudwatch put-metric-alarm --alarm-name "HighMemoryUtilization"
--metric-name MemoryUtilization --namespace AWS/EC2 --statistic Average --period 300
--threshold 80 --comparison-operator GreaterThanThreshold
--dimensions Name=InstanceId,Value=i-0c1d24c351039f6e1 --evaluation-periods 1
--alarm-actions arn:aws:sns:us-east-1:730335276920:alarm-topic --unit Percent

### Set Disc Space Metric
aws cloudwatch put-metric-alarm --alarm-name "LowDiskSpace"
--metric-name DiskSpaceUtilization --namespace AWS/EC2 --statistic Average --period 300
--threshold 20 --comparison-operator GreaterThanThreshold
--dimensions Name=InstanceId,Value=i-0c1d24c351039f6e1 --evaluation-periods 1
--alarm-actions arn:aws:sns:us-east-1:730335276920:alarm-topic --unit Percent

## Step 4 Validate:
aws cloudwatch describe-alarms --alarm-name-prefix "LowDiskSpace"

aws cloudwatch describe-alarms --alarm-name-prefix "HighMemoryUtilization"

aws cloudwatch describe-alarms --alarm-name-prefix "HighCPUUtilization"
