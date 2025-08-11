# Satellite Ground Station - ArgoCD Configuration

Simple GitOps configuration for the satellite ground station cluster.

## 🗂️ **Structure**

```
sat-cluster-argo/
├── apps/
│   └── satellite-apps.yaml      # ArgoCD applications
├── platform.yaml               # Core infrastructure (namespaces, config, storage)
├── cronjob.yaml                # Daily prediction CronJob
└── README.md
```

## 🚀 **Architecture**

**Simple Kubernetes-native approach:**

1. **Daily CronJob** (`cronjob.yaml`) runs `predictor:latest` at 6 AM
2. **Predictor** calculates satellite passes and creates recording Jobs
3. **Recording Jobs** sleep until pass time, then record satellites
4. **Configuration** shared via ConfigMap
5. **Storage** on fast SSD for recordings

## 📡 **Applications**

### **satellite-platform**
- Core infrastructure: namespaces, ConfigMap, storage, RBAC
- Path: `.` (root files)

### **satellite-predictor-cronjob**  
- Daily prediction CronJob
- Creates recording jobs for calculated passes
- Path: `sat-cluster-predictor/k8s`

## 🛰️ **Configuration**

Satellite and ground station config in `platform.yaml`:
- 6 active satellites (NOAA-15, 19, 20, 21, METEOR-M2, M2-2)
- Primary ground station in Lyon area
- 3-day prediction window
- 6-hour update interval

## 🔄 **Workflow**

```
Daily 6 AM
    ↓
CronJob runs predictor:latest
    ↓
Predictor calculates passes
    ↓
Creates recording Jobs with sleep delays
    ↓
Jobs wake up at pass time & record
```

**No Prefect, no complex flows, just Kubernetes!** 🎯
