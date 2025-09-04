# Azure Log Analytics Toolkit

<div align="center">
  
  ![Azure Log Analytics Toolkit](https://img.shields.io/badge/Azure-Log%20Analytics-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
  ![KQL](https://img.shields.io/badge/KQL-Queries-FF6B35?style=for-the-badge&logo=microsoft&logoColor=white)
  ![Security](https://img.shields.io/badge/Security-Monitoring-FF0000?style=for-the-badge&logo=security&logoColor=white)
  ![Operations](https://img.shields.io/badge/Operations-Intelligence-00A86B?style=for-the-badge&logo=operations&logoColor=white)
  
</div>

## 🔍 Overview

A curated collection of Kusto Query Language (KQL) queries for Azure Log Analytics, designed to help security analysts and operations teams monitor their Azure environments effectively. This toolkit focuses on practical, tested queries that address common monitoring and investigation scenarios.

## ✨ What's Included

- **16 Curated KQL Queries** - Carefully selected and tested queries for common monitoring scenarios
- **Security Monitoring** - Authentication failures, security events, and threat detection queries
- **Operational Intelligence** - Performance monitoring, resource utilization, and system health checks
- **Documentation** - Clear explanations and use cases for each query
- **Best Practices** - Guidelines for effective log analysis and monitoring

## 📂 Repository Structure

```
queries/
├── security/           # Security-focused KQL queries
├── operations/         # Operational monitoring queries
├── performance/        # Performance analysis queries
└── examples/          # Sample queries and templates
```

## 🚀 Quick Start

### Prerequisites

- Azure Log Analytics Workspace
- Appropriate RBAC permissions (Log Analytics Reader or higher)
- Basic knowledge of KQL (Kusto Query Language)

### Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/a-ariff/azure-log-analytics-toolkit.git
   cd azure-log-analytics-toolkit
   ```

2. **Browse the queries**
   Navigate to the queries/ directory and explore the categorized KQL queries.

3. **Run your first query**
   Copy any query from the collection and paste it into your Azure Log Analytics workspace.

### Example Query - Failed Authentication Events

```kql
// Get failed authentication events in the last 24 hours
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID == 4625  // Failed logon
| summarize FailedAttempts = count() by Account, Computer, bin(TimeGenerated, 1h)
| where FailedAttempts > 5
| order by FailedAttempts desc
```

## 🎯 Query Categories

### 🛡️ Security Monitoring

- Authentication failures and brute force detection
- Privilege escalation attempts
- Suspicious network activity
- Security event analysis

### ⚙️ Operations & Performance

- Resource utilization monitoring
- Application performance metrics
- System health checks
- Error rate analysis

### 🔍 Troubleshooting

- Log correlation queries
- Performance bottleneck identification
- Service dependency mapping
- Incident investigation helpers

## 🏗️ Enterprise Architecture

This toolkit is designed to support enterprise-scale Azure Log Analytics deployments with sophisticated monitoring architectures. The architecture diagram below showcases how Azure Log Analytics can be implemented across multiple subscriptions with centralized management and comprehensive integration capabilities.

![Azure Log Analytics Enterprise Architecture](assets/enterprise-architecture-diagram.png)

### Key Architecture Components

#### Multi-Subscription Monitoring
- **Centralized Log Analytics Workspace**: A single workspace collecting logs from multiple Azure subscriptions
- **Cross-subscription data collection**: Agents and diagnostic settings configured across different subscription boundaries
- **Unified security monitoring**: Consolidated view of security events and threats across the entire enterprise
- **Scalable resource management**: Distributed resource monitoring with centralized analysis and reporting

#### Centralized Management
- **Single pane of glass**: Unified dashboard for monitoring all enterprise resources
- **Centralized alerting**: Consistent alert rules and notification policies across all subscriptions
- **Role-based access control**: Granular permissions management for different teams and stakeholders
- **Cost optimization**: Centralized log retention policies and data management strategies

#### Integration Capabilities
- **Azure Sentinel integration**: Advanced security analytics and SIEM capabilities
- **Azure Monitor integration**: Comprehensive metrics and logging across all Azure services
- **Third-party SIEM connectivity**: Export capabilities for existing security tools
- **API and PowerShell automation**: Programmatic access for custom integrations and workflows
- **Hybrid connectivity**: On-premises and multi-cloud monitoring through Azure Arc and agents

This enterprise architecture enables organizations to achieve comprehensive visibility, maintain security compliance, and optimize operational efficiency across their entire Azure ecosystem.

## 📚 Learning Resources

- [KQL Quick Reference](https://docs.microsoft.com/en-us/azure/data-explorer/kql-quick-reference)
- [Azure Monitor Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/)
- [KQL Tutorial](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial)

## 🤝 Contributing

Contributions are welcome! Please feel free to:

- Submit new KQL queries with proper documentation
- Report issues with existing queries
- Suggest improvements or optimizations
- Add examples and use cases

Please ensure any contributed queries are:

- Well-documented with clear use cases
- Tested in a real environment
- Follow KQL best practices
- Include appropriate comments

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Ariff Mohamed**

- 📧 Email: ariff@aglobaltec.com
- 💼 [LinkedIn](https://linkedin.com/in/ariff-mohamed)
- 🌐 [Website](https://aglobaltec.com/)

## 🙏 Acknowledgments

- Microsoft Azure Log Analytics team for the powerful platform
- The KQL community for continuous knowledge sharing
- Contributors who help improve this toolkit

---

⭐ If this toolkit helps you in your Azure Log Analytics journey, please consider giving it a star! ⭐

*Built with ❤️ for the Azure community*
