# Operational Excellence
Operational Excellence (OpEx) is about running your services efficiently, reliably, and continuously improving

* Define and Measure Key Metrics
    - SLIs (Service Level Indicators)
    - SLOs (Service Level Objectives)
    - SLAs (Service Level Agreements)

    - Key Metrics
        - Request latency
        - Error rates
        - CPU/memory utilization
        - Deployment success rate
* Monitoring, Logging, & Observability
    - Centralized logs (ELK stack, Loki).
    - Metrics dashboards (Prometheus + Grafana).
    - Distributed tracing (Jaeger, OpenTelemetry) to trace requests across services.
    - Alerts for anomalies
* Automation & Standardization
    - Infrastructure as Code (IaC): Terraform, CloudFormation, Ansible.
    - Automated deployments: CI/CD pipelines, blue/green or canary deployments.
    - Auto-scaling: Adjust resources dynamically.
    - Automated backups & failover: No manual intervention needed.
* Incident & Change Management
    - Incident response plan
    - Postmortems & blameless analysis
    - Change management
* Resilience & Reliability
    - Design for fault tolerance, redundancy, and self-healing.
    - Use retry, circuit breaker, bulkheads, and graceful degradation patterns.
    - Chaos engineering to validate resilience.
* Security & Compliance
    - Code smells
    - Sonar snyk scans
