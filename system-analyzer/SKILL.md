---
name: system-analyzer
description: Comprehensive system audit and optimization for homelab infrastructure. Analyzes Docker containers, cron jobs, systemd services, configurations, disk usage, network topology, and performance metrics. Provides actionable recommendations for automation, resource optimization, and system improvements. Use this whenever the user needs to understand system health, find bottlenecks, identify automation opportunities, optimize resources, or audit infrastructure configuration (Docker, systemd, cron, services, storage, performance).
compatibility: Linux/Unix environment with Docker, systemd, bash
---

# System Analyzer Skill

Comprehensive audit and optimization recommendations for homelab infrastructure.

## Overview

This skill analyzes:
- **Docker containers** — sizing, configuration, networking, restart policies, dependencies
- **Systemd services** — startup order, dependencies, health, restart behavior
- **Cron jobs** — frequency, scheduling optimization, dependency patterns
- **Configuration** — redundancy, inconsistencies, missed opportunities
- **Disk/Storage** — usage patterns, cleanup opportunities, growth trends
- **Performance** — resource utilization, bottlenecks, optimization potential
- **Security** — basic checks (exposed ports, credentials in configs)

## How It Works

The skill follows a structured analysis pipeline:

1. **Data Collection** — Gather system state (containers, services, crons, configs, disks)
2. **Analysis** — Identify patterns, inefficiencies, redundancy, automation gaps
3. **Categorization** — Sort findings into Critical, Warnings, Optimizations
4. **Reporting** — Structured report with specific, actionable recommendations
5. **Verification** — Flag assumptions, note data limitations, suggest next steps

## Input Format

The skill accepts one of three input formats:

### Option A: Auto-Diagnosis (Recommended)
```
/system-analyzer [hostname] [scope]

Examples:
/system-analyzer nest all
/system-analyzer nest docker,cron,services
/system-analyzer . performance
```

- `[hostname]`: nest, pc, laptop, or `.` (current machine)
- `[scope]`: all | docker | cron | services | performance | security | storage

The skill will automatically run diagnostic commands and analyze output.

### Option B: Manual Data Input
```
/system-analyzer --input

Paste outputs of these commands when prompted:
- docker ps -a
- docker network ls
- systemctl list-units --type=service
- crontab -l
- df -h
- ps aux
```

### Option C: Config File Analysis
```
/system-analyzer --config /path/to/config/directory

Analyzes existing configuration files (docker-compose.yml, systemd units, cron jobs, etc.)
```

## Output Report Structure

All reports follow this format:

```
# System Audit: [hostname/date]

## 📊 Executive Summary
- **Overall Health Score**: [0-100]
- **Critical Issues**: N
- **Warnings**: N
- **Optimization Opportunities**: N
- **Estimated Improvement**: [time/resources saved or freed]

## 🔴 CRITICAL ISSUES
[Each issue includes: what it is, why it matters, how to fix, estimated impact]

## 🟡 WARNINGS
[Issues that don't break things but cause problems: inefficiency, bloat, unnecessary complexity]

## ✅ OPTIMIZATIONS
[Opportunities to improve: automation, consolidation, resource efficiency, safety]

## 📈 METRICS & ANALYSIS
- Container efficiency (utilization vs allocated)
- Service dependency graph (startup complexity)
- Cron job scheduling (overlap, resource impact)
- Storage usage breakdown
- Performance bottlenecks (if applicable)

## 🔍 Methodology Notes
- Assumptions made
- Data quality notes
- Recommendations for next steps
```

## Analysis Rules & Logic

### Docker Container Analysis

**Critical Issues:**
- Containers running as `root` without justification
- Containers with `--privileged` flag
- Port conflicts (multiple containers on same port)
- Containers with no restart policy (lost on host reboot)
- Out-of-date images (check build date vs current)

**Warnings:**
- High memory allocation with low utilization (<20%)
- CPU limits not set (can monopolize resources)
- Mounts with poor isolation (e.g., `/` or `/etc`)
- Networks: containers not on any network (using host networking unnecessarily)
- Health checks missing on critical services

**Optimizations:**
- Combine closely-related containers into orchestrated stack
- Consolidate single-container services into multi-container setup if beneficial
- Standardize resource limits across similar services
- Unused networks or volumes
- Redundant configuration (same envvars in multiple containers)
- Docker compose consolidation opportunities

### Systemd Service Analysis

**Critical Issues:**
- Services with `Type=simple` but need cleanup (should be `Type=oneshot`)
- Circular dependencies (A requires B, B requires A)
- Services failing to start on boot (enabled but always failing)
- Critical services missing `Restart=` policy
- Missing user privileges (running as root when unnecessary)

**Warnings:**
- Long `ExecStart` scripts (should be wrapped in script file)
- No `StandardError=journal` (logging not centralized)
- Multiple services with same binary/purpose (unclear which is primary)
- Services with no documentation (no `Description=`)
- Dependency chains >3 levels deep (startup order complexity)

**Optimizations:**
- Consolidate related services into single unit with socket activation
- Add health checks via `ExecHealthCheck=`
- Improve startup parallelization (reduce `After=` dependencies)
- Move configuration to proper config files (not hardcoded in unit)
- Add `StandardOutput=journal` for centralized logging

### Cron Job Analysis

**Critical Issues:**
- Cron jobs running as `root` without clear need
- Jobs without proper error handling (failing silently)
- Overlapping jobs (high frequency jobs blocking each other)
- Jobs with no timeout (can hang and stack up)
- Jobs modifying critical system files without backup

**Warnings:**
- Jobs running too frequently (e.g., every 1 minute)
- No logging (can't debug failures)
- Jobs referencing relative paths (breaks if cron location changes)
- Unrelated jobs in same cron entry (hard to maintain)
- High variance in execution time (suggests external dependency)

**Optimizations:**
- Consolidate related cron jobs (multiple small jobs → one orchestrated job)
- Replace frequent polling with event-driven approach (systemd.path, inotify)
- Move cron logic to systemd timer units
- Add structured logging and monitoring
- Optimize scheduling to avoid peak hours

### Configuration Analysis

**Critical Issues:**
- Hardcoded credentials (passwords, API keys)
- Inconsistent service names across configs (confusing)
- Duplicate configuration sections
- Missing `.env` files or environment variable usage

**Warnings:**
- Configuration drift (manual changes not in version control)
- Inconsistent naming schemes (camelCase vs snake_case)
- Magic numbers without explanation
- Hardcoded paths (breaks if moved)
- Missing CHANGELOG or version tracking

**Optimizations:**
- Centralize common configuration
- Use `.env` files and environment variables
- Template repetitive configs
- Add configuration validation script
- Document assumptions and dependencies

### Storage Analysis

**Critical Issues:**
- Partition usage >95% (no room for growth)
- Outdated backups (>30 days old, if backups exist)
- Untracked large files (>1GB, unknown purpose)
- Disk I/O bottleneck (iowait >50%)

**Warnings:**
- Unused/orphaned container images (taking space)
- Large log files not rotated
- Temporary files accumulating
- Inefficient mount points (multiple disks, unclear strategy)

**Optimizations:**
- Log rotation and cleanup policies
- Cleanup orphaned Docker objects
- Archive old data
- Implement disk monitoring/alerts
- Optimize backup strategy

### Performance Analysis

**Critical Issues:**
- CPU utilization consistently >80%
- Memory utilization >90%
- Services swapping (very slow)
- High iowait indicating I/O bottleneck

**Warnings:**
- Variable performance (suggests resource contention)
- No performance monitoring in place
- Services sharing CPU unfairly (one monopolizing)
- Memory leaks (process growing over time)

**Optimizations:**
- CPU/memory limits and requests
- Service prioritization (nice/priority levels)
- Cgroup limits for container groups
- Add performance monitoring (Prometheus, Netdata)
- Identify and eliminate resource contention

## Example Analysis Session

**Input:**
```
/system-analyzer nest all
```

**Processing:**
1. Connect to nest (SSH if remote)
2. Run: `docker ps -a`, `docker network ls`, `docker stats`
3. Run: `systemctl list-units --type=service`, `systemctl status`
4. Run: `crontab -l` (for root and hushus user)
5. Run: `df -h`, `du -sh /*`, `lsof` (open files)
6. Analyze: cross-service dependencies, resource patterns, anomalies
7. Generate: structured report with categories and priorities

**Output:**
```markdown
# System Audit: nest (2026-04-08)

## 📊 Executive Summary
- **Overall Health Score**: 78/100
- **Critical Issues**: 1
- **Warnings**: 4
- **Optimization Opportunities**: 7
- **Estimated Improvement**: Free 2GB RAM, reduce boot time 30s

## 🔴 CRITICAL ISSUES

### 1. Docker Container: caddy without restart policy
**What**: caddy-caddy-1 has `restart_policy: null`
**Why**: If caddy crashes, it won't auto-restart → services unreachable
**Fix**: Add `restart_policy: unless-stopped` to docker-compose.yml
**Impact**: Prevents hours of downtime from single container crash
```

## Key Principles

### 1. **Actionable Recommendations**
Every issue includes:
- **What** — What is the problem?
- **Why** — Why does it matter?
- **How** — Specific steps to fix it
- **Impact** — What improves?

NOT just "this looks wrong" — always provide solution.

### 2. **Context-Aware Analysis**
The skill understands:
- Homelab vs enterprise (different standards)
- Development vs production environments
- User skill level (novice vs advanced)
- Historical context (what changed recently)

### 3. **False Positive Avoidance**
- Flag assumptions: "Assuming cron job X should run at this frequency"
- Note exceptions: "root cron jobs acceptable for certain tasks"
- Request confirmation: "Is service Y intended to be critical?"

### 4. **Prioritization by Impact**
- **Critical** = downtime, security, data loss risk
- **Warnings** = inefficiency, complexity, tech debt
- **Optimizations** = improvements, not requirements

## Limitations & Next Steps

This skill analyzes static state. It does NOT:
- Monitor trends over time (need persistent metrics)
- Test recovery procedures (requires safe environment)
- Benchmark against similar systems (no comparison baseline)
- Make security audit claims (partial analysis only)

**Recommended follow-ups:**
- Set up monitoring (Prometheus, Grafana, Netdata)
- Run configuration validation (ansible-lint, docker-compose validation)
- Test failover scenarios (controlled environment)
- Schedule regular audits (quarterly or after major changes)

## Usage Examples

```bash
# Analyze Docker and cron on current machine
/system-analyzer . docker,cron

# Full audit of nest
/system-analyzer nest all

# Security-focused analysis
/system-analyzer laptop security

# Storage and performance analysis
/system-analyzer pc storage,performance

# Manual input (when can't run commands)
/system-analyzer --input
< paste docker ps output >
< paste systemctl output >
... etc
```

## Related Skills

- **simplify** — After audit, use to clean up/refactor code
- **mcp-builder** — To create monitoring/alerting infrastructure
- **skill-creator** — To build custom analysis for your specific setup
