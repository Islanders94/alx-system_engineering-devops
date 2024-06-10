**Issue Summary** e-com disaster

**Duration:** The outage lasted from 10:30 AM to 12:45 PM EDT on June 8, 2024.

**Impact:** The outage affected the main e-commerce website, causing it to be completely inaccessible to users. Approximately 80% of our user base experienced issues, with an estimated 20,000 users unable to browse or make purchases during this period.

**Root Cause:** The root cause of the outage was a misconfiguration in the database replication setup, which led to a cascade failure in the database cluster.

---

**Timeline**

- **10:30 AM** - Issue detected by monitoring alerts indicating high error rates and slow response times.
- **10:35 AM** - Engineering team notified through the on-call system.
- **10:40 AM** - Initial investigation focused on web server performance and load balancer configurations.
- **11:00 AM** - Misleading path: Assumed issue was due to a recent deployment; rolled back the deployment with no improvement.
- **11:20 AM** - Escalated to the Database team after identifying abnormal database read/write latency.
- **11:35 AM** - Database team discovered replication lag and inconsistencies.
- **12:00 PM** - Identified that a misconfiguration in the replication setup caused the primary database to become overloaded.
- **12:15 PM** - Began reconfiguring the database replication and balancing the load.
- **12:30 PM** - System started stabilizing, with the website becoming partially accessible.
- **12:45 PM** - Full resolution, with the website fully operational and all services restored.

---

**Root Cause and Resolution**

**Root Cause:** The issue stemmed from a recent change in the database replication configuration. A parameter was incorrectly set, causing the secondary databases to fall behind in replication. This lag increased the load on the primary database, which eventually became overwhelmed and led to a failure in handling incoming requests.

**Resolution:** The immediate resolution involved correcting the misconfiguration in the replication setup. The database team reconfigured the replication parameters to ensure proper synchronization between the primary and secondary databases. Additionally, they redistributed the read load more evenly across the cluster to prevent future overloads.

---

**Corrective and Preventative Measures**

**Improvements/Fixes:**

1. **Review and Audit Configuration Changes:** Implement a stricter review process for database configuration changes, including peer reviews and automated checks.
2. **Enhanced Monitoring:** Add detailed monitoring for database replication status, including alerts for lag and inconsistencies.
3. **Load Testing:** Conduct regular load testing to identify potential bottlenecks in the database cluster and other critical components.
4. **Disaster Recovery Drills:** Perform regular drills to ensure rapid response and recovery during outages.

**TODO List:**

- Patch the database replication configuration system to include safeguards against misconfigurations.
- Develop and deploy additional monitoring tools to track replication health and performance.
- Schedule bi-weekly load testing sessions to evaluate the robustness of the database and web server infrastructure.
- Conduct a comprehensive review of the incident response process and update the runbooks to include steps for handling database-related outages.
- Provide training sessions for the engineering and database teams on best practices for configuration management and incident response.

---

To make this postmortem a bit more engaging, let's add a little humor. Imagine our database cluster as a bustling kitchen. One chef (our primary database) was overwhelmed because the sous chefs (secondary databases) were not getting the ingredients (data) fast enough due to a broken conveyor belt (replication misconfiguration). As a result, the chef had a meltdown (outage), and no orders (user requests) were coming out of the kitchen. We fixed the conveyor belt, distributed the tasks properly, and voilà, the kitchen was back to serving delicious meals (restored service) in no time!

