# DevOps Intelligence

## Purpose
Comprehensive DevOps framework covering CI/CD, infrastructure, observability, and operational excellence.

## CI/CD Pipeline

### Pipeline Stages

```
PIPELINE:
  Commit → Build → Test → Security Scan → Deploy to Staging → E2E Tests → Deploy to Production
```

### Stage Definitions

| Stage | Purpose | Gates |
|-------|---------|-------|
| **Build** | Compile, package artifacts | Zero build errors, version tagged |
| **Unit Test** | Fast feedback on logic | 80%+ coverage, all tests pass |
| **Integration Test** | Component interactions | Service tests pass, DB migrations work |
| **Security Scan** | Vulnerability detection | No critical/high vulns |
| **Code Quality** | Maintainability | Lint passes, complexity thresholds |
| **Staging Deploy** | Pre-prod validation | Infrastructure as Code valid |
| **E2E Test** | User flow validation | Critical paths pass |
| **Production Deploy** | Live release | Manual approval or automated canary |

### Deployment Strategies

| Strategy | Description | Risk | Recovery |
|----------|-------------|------|----------|
| **Rolling** | Gradual instance replacement | Low | Automatic rollback |
| **Blue/Green** | Parallel environments, switch | Low | Switch back |
| **Canary** | Small % traffic, expand | Very Low | Automatic revert |
| **Feature Flag** | Code deployed, feature off | Minimal | Toggle off |
| **A/B Deploy** | Route segments to versions | Low | Route adjustment |

### Recommended: Canary with Feature Flags

```
DEPLOYMENT:
  1. Deploy with feature OFF (flag)
  2. Verify health checks
  3. Enable for 1% users (canary)
  4. Monitor error rate, latency (5 min)
  5. Gradually increase: 10% → 50% → 100%
  6. At each step: monitor, auto-rollback if error rate > threshold
```

## Infrastructure as Code

### Principles

1. **Version controlled** - All infrastructure in Git
2. **Immutable** - Replace, don't modify
3. **Idempotent** - Same input = same output
4. **Testable** - Plan before apply, validate
5. **Modular** - Reusable components

### State Management

```json
{
  "state_management": {
    "remote_state": true,
    "state_locking": true,
    "encryption": true,
    "backup": true,
    "workspace_isolation": true
  }
}
```

## Observability Stack

### Three Pillars

**Metrics (What is happening?):**
```json
{
  "metric_types": {
    "infrastructure": ["cpu", "memory", "disk", "network"],
    "application": ["requests_per_second", "latency_p95", "error_rate"],
    "business": ["orders_per_minute", "conversion_rate", "revenue"]
  },
  "retention": "15 months",
  "aggregation": "1 min (recent) → 5 min → 1 hour → 1 day"
}
```

**Logs (Why is it happening?):**
```json
{
  "log_levels": ["DEBUG", "INFO", "WARN", "ERROR", "FATAL"],
  "structure": "JSON",
  "required_fields": ["timestamp", "level", "service", "message", "trace_id"],
  "retention": "30 days hot, 1 year cold",
  "correlation": "trace_id across services"
}
```

**Traces (Where is it happening?):**
```json
{
  "trace_format": "OpenTelemetry",
  "sampling": "1% default, 100% for errors",
  "spans": ["incoming_request", "database_query", "external_api_call", "cache_lookup"],
  "max_depth": 10
}
```

### Alerting Rules

```yaml
alerts:
  - name: high_error_rate
    condition: error_rate > 1% for 5 minutes
    severity: critical
    action: page_oncall
    
  - name: high_latency
    condition: p95_latency > 500ms for 10 minutes
    severity: warning
    action: slack_notification
    
  - name: low_availability
    condition: availability < 99.9% over 1 hour
    severity: critical
    action: page_oncall
    
  - name: disk_full
    condition: disk_usage > 85%
    severity: warning
    action: slack_notification
```

### Dashboard Templates

| Dashboard | Panels |
|-----------|--------|
| **Service Overview** | RPS, latency (p50/p95/p99), error rate, throughput |
| **Infrastructure** | CPU, memory, disk, network, instance count |
| **Database** | Query time, connections, replication lag, slow queries |
| **Business** | Conversion funnel, revenue, user activity |
| **Error Analysis** | Error rate by endpoint, error types, stack traces |

## Operational Runbooks

### On-Call Response

```
ON_CALL_RESPONSE:
  1. Acknowledge alert (within 5 min)
  2. Assess severity (user impact? data loss?)
  3. Mitigate (rollback, scale up, feature flag off)
  4. Communicate (status page, stakeholders)
  5. Investigate root cause
  6. Fix permanently
  7. Post-incident review
```

### Incident Severity

| Severity | Impact | Response Time | Actions |
|----------|--------|---------------|---------|
| **S1** | Complete outage, data loss | Immediate | All hands, war room |
| **S2** | Major feature broken | 15 minutes | On-call + domain expert |
| **S3** | Degraded performance | 1 hour | On-call |
| **S4** | Cosmetic/minor | Next business day | Ticket |

## SRE Practices

### Error Budgets

```
ERROR_BUDGET:
  Availability target: 99.9%
  Error budget: 0.1% downtime = 43.8 min/month
  
  Policy:
    - If budget > 50% remaining: normal velocity
    - If budget < 50% remaining: reduce risky changes
    - If budget exhausted: freeze non-critical deploys
```

### SLIs/SLOs/SLAs

| Level | Definition | Example |
|-------|------------|---------|
| **SLI** | Service Level Indicator | Request latency |
| **SLO** | Service Level Objective | p95 < 200ms |
| **SLA** | Service Level Agreement | 99.9% availability (customer contract) |

## Health Checks

```json
{
  "health_checks": {
    "liveness": {
      "purpose": "Is the process running?",
      "endpoint": "/health/live",
      "response": "200 if process up"
    },
    "readiness": {
      "purpose": "Can the service accept traffic?",
      "endpoint": "/health/ready",
      "checks": ["database", "cache", "critical_dependencies"]
    },
    "startup": {
      "purpose": "Has the service finished initializing?",
      "endpoint": "/health/startup",
      "checks": ["migrations", "warmup", "config_loaded"]
    }
  }
}
```

## Environment Strategy

| Environment | Purpose | Data | Access |
|-------------|---------|------|--------|
| **Local** | Development | Synthetic | Developer |
| **Dev** | Integration | Synthetic | Team |
| **Staging** | Pre-prod validation | Production-like | Team + QA |
| **Production** | Live traffic | Real | Limited |

## Disaster Recovery

### RPO/RTO Definitions

| Tier | RPO (Data Loss) | RTO (Downtime) | Method |
|------|-----------------|----------------|--------|
| **Tier 1** | 0 | < 1 hour | Multi-region active-active |
| **Tier 2** | < 1 hour | < 4 hours | Hot standby |
| **Tier 3** | < 24 hours | < 24 hours | Cold backup |

### DR Checklist
- [ ] Backups tested (restore procedure validated)
- [ ] Failover documented and tested
- [ ] Data replication verified
- [ ] DNS failover configured
- [ ] Communication plan ready
- [ ] Post-incident recovery validated
