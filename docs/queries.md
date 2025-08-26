# KQL Query Examples

This document contains sample Kusto Query Language (KQL) queries for Azure Log Analytics, organized by functional areas to help with security monitoring, performance analysis, and operational intelligence.

## 🛡️ Security Monitoring

### Failed Login Attempts
Detect failed authentication attempts across your environment:

```kql
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID == 4625  // Failed logon
| summarize FailedAttempts = count() by Account, Computer, IpAddress
| where FailedAttempts >= 5
| order by FailedAttempts desc
```

### Privileged Account Activity
Monitor activities involving privileged accounts:

```kql
SecurityEvent
| where TimeGenerated > ago(7d)
| where AccountType == "Admin" or Account has "admin"
| where EventID in (4672, 4673, 4674)  // Special privileges assigned/used
| summarize Activities = count() by Account, Computer, Activity
| order by Activities desc
```

### Unusual Process Execution
Detect suspicious process execution patterns:

```kql
SecurityEvent
| where TimeGenerated > ago(1d)
| where EventID == 4688  // Process creation
| extend ProcessName = extract(@"([^\\]+)$", 1, NewProcessName)
| summarize ExecutionCount = count() by ProcessName, Computer
| where ExecutionCount < 5  // Rare processes
| order by ExecutionCount asc
```

## 📊 Performance Monitoring

### High CPU Utilization
Identify systems with consistently high CPU usage:

```kql
Perf
| where TimeGenerated > ago(4h)
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where InstanceName == "_Total"
| summarize AvgCPU = avg(CounterValue) by Computer, bin(TimeGenerated, 15m)
| where AvgCPU > 80
| order by TimeGenerated desc
```

### Memory Usage Analysis
Analyze memory consumption patterns:

```kql
Perf
| where TimeGenerated > ago(24h)
| where ObjectName == "Memory" and CounterName == "% Committed Bytes In Use"
| summarize AvgMemory = avg(CounterValue), MaxMemory = max(CounterValue) by Computer
| where MaxMemory > 85
| order by MaxMemory desc
```

### Disk Space Monitoring
Monitor disk space utilization:

```kql
Perf
| where TimeGenerated > ago(1h)
| where ObjectName == "LogicalDisk" and CounterName == "% Free Space"
| where InstanceName != "_Total"
| summarize FreeSpace = avg(CounterValue) by Computer, InstanceName
| where FreeSpace < 15
| order by FreeSpace asc
```

## 🌐 Network Security

### Network Connection Analysis
Analyze network connections and identify anomalies:

```kql
WireData
| where TimeGenerated > ago(2h)
| summarize Connections = count() by RemoteIP, Direction, ProcessName
| where Connections > 100
| order by Connections desc
```

### Firewall Blocks
Monitor blocked network traffic:

```kql
AzureNetworkAnalytics_CL
| where TimeGenerated > ago(1d)
| where FlowStatus_s == "D"  // Denied
| summarize BlockedConnections = count() by SrcIP_s, DestIP_s, DestPort_d
| where BlockedConnections >= 10
| order by BlockedConnections desc
```

## 📱 Application Monitoring

### Application Error Analysis
Monitor application errors and exceptions:

```kql
AppServiceConsoleLogs
| where TimeGenerated > ago(6h)
| where ResultDescription contains "error" or ResultDescription contains "exception"
| summarize ErrorCount = count() by AppName = split(_ResourceId, "/")[-1], bin(TimeGenerated, 30m)
| order by TimeGenerated desc
```

### HTTP Response Code Analysis
Analyze web application response patterns:

```kql
AppServiceHTTPLogs
| where TimeGenerated > ago(4h)
| summarize RequestCount = count() by HttpStatusCode = ScStatus, bin(TimeGenerated, 15m)
| where HttpStatusCode >= 400
| order by TimeGenerated desc
```

## 💾 System Health

### Service Status Monitoring
Monitor critical Windows services:

```kql
Event
| where TimeGenerated > ago(2h)
| where Source == "Service Control Manager"
| where EventID in (7034, 7035, 7036)  // Service stopped/started events
| project TimeGenerated, Computer, RenderedDescription
| order by TimeGenerated desc
```

### System Boot Analysis
Track system boot events:

```kql
Event
| where TimeGenerated > ago(7d)
| where EventID == 6005  // Event log service started
| summarize BootCount = count() by Computer, bin(TimeGenerated, 1d)
| order by TimeGenerated desc
```

## 🔍 Threat Hunting

### PowerShell Activity
Monitor PowerShell execution for suspicious activity:

```kql
Event
| where TimeGenerated > ago(24h)
| where Source == "PowerShell" and EventID == 4103
| extend CommandLine = tostring(EventData.Data)
| where CommandLine has_any ("Invoke-", "Download", "Base64", "Encode")
| project TimeGenerated, Computer, CommandLine
| order by TimeGenerated desc
```

### Registry Modifications
Detect suspicious registry changes:

```kql
SecurityEvent
| where TimeGenerated > ago(12h)
| where EventID == 4657  // Registry value modified
| where ObjectName has_any ("Run", "RunOnce", "CurrentVersion")
| project TimeGenerated, Computer, Account, ObjectName, ProcessName
| order by TimeGenerated desc
```

## 📈 Custom Metrics

### User Activity Summary
Summarize user activity patterns:

```kql
SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType == 0  // Successful sign-ins
| summarize 
    SignIns = count(),
    UniqueApps = dcount(AppDisplayName),
    UniqueIPs = dcount(IPAddress)
    by UserPrincipalName
| where SignIns > 10
| order by SignIns desc
```

### Resource Utilization Overview
Get a comprehensive view of resource usage:

```kql
Perf
| where TimeGenerated > ago(1h)
| where CounterName in ("% Processor Time", "% Committed Bytes In Use")
| summarize AvgValue = avg(CounterValue) by Computer, CounterName
| evaluate pivot(CounterName, AvgValue)
| extend HealthStatus = iff(["% Processor Time"] > 80 or ["% Committed Bytes In Use"] > 85, "High", "Normal")
| order by HealthStatus desc
```

## 💡 Tips for Effective Querying

1. **Use time filters**: Always include appropriate time filters to improve performance
2. **Limit results**: Use `take` or `top` to limit result sets during development
3. **Index optimization**: Filter on indexed columns early in your query
4. **Summarization**: Use `summarize` to aggregate data and reduce result size
5. **Performance**: Test queries with smaller time ranges before expanding scope

## 🔗 Useful KQL Functions

- `ago()` - Relative time references
- `between()` - Time range filtering
- `extract()` - Regular expression extraction
- `split()` - String splitting
- `parse()` - Parse structured data
- `join` - Combine data from multiple tables
- `union` - Combine results from multiple queries

---

*For more advanced queries and custom solutions, check the main repository documentation.*
