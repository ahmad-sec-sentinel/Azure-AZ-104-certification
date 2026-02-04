# 🌍 Global High-Availability Web Application
## Azure Traffic Manager Multi-Region Deployment (Performance Routing)

**Author:** Ahmad Saalik Hussain | **DevOps & Cloud Engineer**  
**Date:** February 2026 | **Skills Demonstrated:** Multi-region architecture, Performance-based global routing, Data residency compliance

### 📋 Executive Summary
Deployed globally distributed web app across **West US** and **South India** using **Azure Traffic Manager Performance routing**. Automatically routes users to **lowest-latency endpoint** while respecting regional data boundaries—critical for GCC compliance.

## 🔧 Architecture Overview

```mermaid
graph TB
    A[🌐 Global Users] -->|DNS Resolution| B(Traffic Manager<br/>Performance Routing)
    B -->|HTTP Health Probes<br/>port 80, /index.html| C(( ))
    C --> D[West US VM<br/>Latency: 250ms<br/>Apache2 + Custom HTML]
    C --> E[South India VM<br/>Latency: 45ms<br/>Apache2 + Custom HTML]
    
    style B fill:#1f77b4
    style D fill:#ff7f0e
    style E fill:#2ca02c

- **Routing Method:** Performance (lowest latency wins)
- **Health Probes:** HTTP GET / every 30s
- **Result:** South India selected (45ms from Faridabad)


### 🎯 Traffic Manager Routing Methods

| **Method** | **Crisp Logic** | **GCC Use Case** |
|------------|-----------------|------------------|
| **Performance** ⭐ | Lowest latency | Qatar users → UAE Central (25ms) |
| **Priority** | Primary → Failover | KSA Primary → UAE Backup |
| **Weighted** | Traffic % split | Blue/Green deployments |
| **Geographic** | By country | KSA → Saudi region only |
| **MultiValue** | Multiple healthy IPs | Mobile app server pools |
| **Subnet** | Client IP ranges | Corp VPN → Private endpoints |

### 🚀 Implementation Steps
1. **VMs:** Ubuntu in West US + South India (Public IP + DNS labels)
2. **Apache:** Custom `index.html` on both → HTTP 80 probes
3. **Traffic Manager:** `global-web-tm` → **Performance** routing → South India wins (45ms from Delhi)

### 💡 Real-World Impact (GCC Data Residency Compliant)

**Static Content / Edge Apps** (your lab pattern):
- **Qatar E-commerce CDN**: Routes to UAE Central (25ms)—static assets served fast, dynamic data stays local.[web:25]
- **UAE Media Portals**: Performance routes GCC traffic to UAE North; fallback to Europe for global reach.
- **Saudi Logistics Dashboards**: Pre-2026 Saudi region, routes to UAE Central for non-sensitive tracking (Vision 2030 compliant).[web:20]

**Sensitive Data Pattern** (Geographic routing):
- KSA banking → Saudi region only (PDPL enforcement).
- Hybrid: Traffic Manager → Azure Front Door → Regional storage.

**Why Compliant**: Performance for **stateless/low-sensitivity** (your web lab); Geographic for regulated data.

### ⚖️ Traffic Manager vs Load Balancer
| Aspect | Load Balancer | Traffic Manager |
|--------|---------------|-----------------|
| Scope | Regional | **Global** |
| Optimization | None | **Latency + Geo** |
| GCC DR | Limited | Cross-region compliant |

### 🏆 Value Proposition
- **GCC Ready**: UAE Central primary for Qatar/UAE traffic
- **Scalable*
- **Cost-Optimized**: Route only to healthy regions

**🔗 Demo:** `global-web-tm.trafficmanager.net` | **IaC:** [GitHub]
