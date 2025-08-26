# Azure Log Analytics Toolkit Documentation

Welcome to the Azure Log Analytics Toolkit documentation. This comprehensive resource provides enterprise-grade Azure Log Analytics queries, workbooks, and monitoring solutions for security insights and operational intelligence.

## 🔍 What's Inside

### KQL Queries
Powerful Kusto Query Language (KQL) queries designed for:
- Security monitoring and threat detection
- Performance analysis and troubleshooting  
- Compliance and audit reporting
- Operational intelligence and insights

### Azure Workbooks
Interactive dashboards and visualizations including:
- Security operations center (SOC) dashboards
- Performance monitoring workbooks
- Compliance reporting templates
- Custom visualization templates

### Alert Rules
Preconfigured alert rules for:
- Security incidents and anomalies
- Performance degradation detection
- System health monitoring
- Compliance violations

## 📚 Getting Started

1. **[Sample Queries](./queries.md)** - Explore our collection of KQL queries with examples
2. **Workbooks** - Deploy interactive monitoring dashboards
3. **Alerts** - Set up proactive monitoring and notifications

## 🚀 Quick Start

```kql
// Example: Recent security events
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID in (4625, 4648, 4719)
| summarize Count = count() by Computer, EventID
| order by Count desc
```

## 📖 Documentation Sections

- **[Query Examples](./queries.md)** - Comprehensive KQL query library
- **Workbook Templates** - Ready-to-deploy monitoring dashboards
- **Alert Configurations** - Proactive monitoring setup guides
- **Best Practices** - Implementation guidelines and recommendations

## 🔧 Prerequisites

- Azure Log Analytics workspace
- Azure Monitor access
- Appropriate RBAC permissions
- Basic understanding of KQL

## 📞 Support

For questions, issues, or contributions, please visit our [GitHub repository](https://github.com/a-ariff/azure-log-analytics-toolkit).

---

*Last updated: August 2025*
