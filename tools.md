
---

# **The Observability & Incident Management Stack**

## **Understanding the Problem First**

Imagine you're running a production application:
- 50 microservices
- Running on 100+ servers
- Serving 1 million users
- Deployed across multiple regions

**Questions you need to answer 24/7:**
1. Is everything running smoothly? ✅
2. Are users experiencing issues? 🔍
3. If something breaks, who gets alerted? 🚨
4. How do we communicate status to users? 📢
5. What caused the issue? 📊

**This is where these tools come in:**

```
┌─────────────────────────────────────────────────────────────┐
│                   OBSERVABILITY STACK                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  DATADOG          → Monitoring, Metrics, Logs, Traces       │
│  Site24x7         → External monitoring, uptime checks      │
│  OpsGenie         → Alerting, On-call management            │
│  Statuspage.io    → Public status page for users            │
│  CloudHealth      → Cloud cost optimization                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

Let me explain each one:

---

# **1. DATADOG - The Observability Platform**

## **What is Datadog?**

**Think of Datadog as your application's "security camera system + health monitor + detective"**

It gives you visibility into:
- **Infrastructure**: Servers, containers, databases
- **Applications**: Performance, errors, slow requests
- **Logs**: What's happening in your code
- **Network**: Traffic between services
- **User Experience**: How fast pages load for real users

---

## **Datadog Core Components**

### **A) Metrics (Time-Series Data)**

**What are metrics?**
Numbers that change over time: CPU usage, memory, request count, error rate, latency

```
Example metrics:
- system.cpu.usage: 45% → 52% → 48% → 60%
- web.requests.count: 1000 → 1200 → 980 → 1100
- database.query.latency: 50ms → 120ms → 45ms → 300ms (🚨)
- api.errors.rate: 0.1% → 0.2% → 5% (🚨🚨🚨)
```

**How Datadog collects metrics:**

```
┌──────────────────┐
│   Your Server    │
│                  │
│  ┌────────────┐  │         ┌──────────────┐
│  │Datadog     │  │────────>│  Datadog     │
│  │Agent       │  │ Sends   │  Platform    │
│  │(runs local)│  │ metrics │  (Cloud)     │
│  └────────────┘  │         └──────────────┘
│                  │
│  App, OS, etc.   │
└──────────────────┘
```

**The Datadog Agent:**
```bash
# Installed on each server/container
# Runs in background
# Collects metrics every 15 seconds by default

# On Linux:
sudo apt-get install datadog-agent

# Configure with API key
sudo vi /etc/datadog-agent/datadog.yaml
# api_key: your-api-key-here

# Start agent
sudo systemctl start datadog-agent

# Check status
sudo datadog-agent status
```

---

### **B) APM (Application Performance Monitoring)**

**What is APM?**
Traces requests as they flow through your application

**Example: User clicks "Buy" button**

```
Request Flow (Distributed Trace):

1. Web Browser
      │
      │ 120ms
      ▼
2. Load Balancer
      │
      │ 5ms
      ▼
3. Frontend Service (Python)
      │
      │ 80ms (calling API)
      ▼
4. Backend API Service (Node.js)
      │
      ├─> 40ms → Database Query (PostgreSQL)
      │
      ├─> 25ms → Cache Check (Redis)
      │
      └─> 60ms → Payment Service (External API)

Total: 330ms

Datadog shows: "Payment Service is slow! 60ms (usually 15ms)"
```

**How to instrument your app:**

```python
# Python Flask example
from ddtrace import tracer
from flask import Flask

app = Flask(__name__)

@app.route('/api/checkout')
def checkout():
    # Datadog automatically traces this
    user = get_user_from_db()  # Traced
    process_payment(user)       # Traced
    send_email(user)           # Traced
    return "Success"
```

**What you see in Datadog APM:**

```
Service Map:
┌──────────┐      ┌──────────┐      ┌──────────┐
│ Frontend │─────>│ Backend  │─────>│ Database │
│  (Web)   │      │   (API)  │      │   (PG)   │
└──────────┘      └──────────┘      └──────────┘
   120ms             80ms              40ms
   99.9% OK         99.5% OK         99.99% OK
   
Traces (individual requests):
├─ Request ID: abc123
│  └─ Total: 330ms
│     ├─ Frontend: 120ms
│     ├─ Backend: 80ms
│     │  ├─ DB Query: 40ms
│     │  ├─ Redis: 25ms
│     │  └─ Payment: 60ms ⚠️ (SLOW)
│     └─ Response: 5ms
```

---

### **C) Logs (What's Happening)**

**Centralized log management**

Instead of SSH-ing into 100 servers to check logs, send all logs to Datadog:

```bash
# Your application logs
2024-02-16 10:23:45 INFO User 12345 logged in
2024-02-16 10:23:46 ERROR Database connection timeout
2024-02-16 10:23:47 WARN Retry attempt 1/3
2024-02-16 10:23:48 ERROR Payment failed: card declined
2024-02-16 10:23:49 INFO Email sent to user@example.com
```

**Datadog Log Pipeline:**

```
Application
    │
    │ Writes logs
    ▼
Log File (/var/log/app.log)
    │
    │ Datadog Agent reads
    ▼
Datadog Platform
    │
    │ Parse, index, search
    ▼
Datadog UI
    │
    └─> Search: "ERROR" in last 1 hour
        Results: 47 errors
        Top error: "Database connection timeout" (23 times)
```

**Log querying in Datadog:**

```
Query examples:
- status:error service:api-backend
- "payment failed" @user.id:12345
- @http.status_code:500 env:production
- source:nginx @http.url_details.path:/checkout
```

**Setting up log collection:**

```yaml
# datadog.yaml
logs_enabled: true

# For Docker containers
docker run -d \
  --name datadog-agent \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v /proc/:/host/proc/:ro \
  -v /sys/fs/cgroup/:/host/sys/fs/cgroup:ro \
  -e DD_API_KEY=<YOUR_API_KEY> \
  -e DD_LOGS_ENABLED=true \
  -e DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true \
  datadog/agent:latest
```

---

### **D) Dashboards (Visualizing Data)**

**Creating a dashboard in Datadog:**

```
Dashboard: "Production API Health"

┌─────────────────────────────────────────────────────────┐
│  [Last 1 hour]                          [Auto-refresh]  │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌────────────────┐  ┌────────────────┐                │
│  │ Requests/sec   │  │ Error Rate     │                │
│  │                │  │                │                │
│  │     1,234      │  │     0.12%      │                │
│  │  ▲ +5% (1h)    │  │  ▼ -0.03%     │                │
│  └────────────────┘  └────────────────┘                │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Response Time (p95)                             │  │
│  │  ┌─────────────────────────────────────────┐    │  │
│  │  │     /\    /\                  /\        │    │  │
│  │  │    /  \  /  \   /\    /\    /  \       │    │  │
│  │  │___/____\/____\_/__\__/__\__/____\______│    │  │
│  │  │ 10am  11am  12pm  1pm   2pm   3pm      │    │  │
│  │  │ 120ms 150ms 140ms 180ms 200ms 160ms    │    │  │
│  │  └─────────────────────────────────────────┘    │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Top Errors (Last hour)                          │  │
│  │  1. Database timeout (23 times)                  │  │
│  │  2. Payment gateway error (12 times)             │  │
│  │  3. Invalid user token (8 times)                 │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

**How to build dashboards:**

1. **Go to Datadog UI** → Dashboards → New Dashboard
2. **Add widgets:**
   - Timeseries graph (CPU, memory over time)
   - Query value (single number: current error rate)
   - Top list (top 10 slowest endpoints)
   - Heatmap (latency distribution)
   - Service map (architecture visualization)

3. **Example widget configuration:**
```
Widget: Timeseries
Metric: avg:system.cpu.user by {host}
Filter: env:production
Visualization: Line graph
Time range: Last 4 hours
```

---

### **E) Monitors (Alerting)**

**Monitors = Automated alerts when things go wrong**

**Example monitor: "High API error rate"**

```
Monitor Configuration:
┌────────────────────────────────────────────────┐
│ Name: API Error Rate Too High                  │
│                                                 │
│ Metric: sum:api.errors{env:production}         │
│         / sum:api.requests{env:production}      │
│                                                 │
│ Alert threshold:   > 5%                         │
│ Warning threshold: > 2%                         │
│                                                 │
│ Evaluation: Check every 1 minute                │
│                                                 │
│ Alert after: 3 consecutive breaches             │
│ (Reduces false alarms)                          │
│                                                 │
│ Notify:                                         │
│  - OpsGenie (pages on-call engineer)           │
│  - Slack #alerts channel                        │
│  - Email: sre-team@company.com                  │
│                                                 │
│ Message:                                        │
│  "🚨 API error rate is {{value}}%              │
│   Threshold: {{threshold}}%                     │
│   Check: https://app.datadoghq.com/dash/..."   │
└────────────────────────────────────────────────┘
```

**Common monitor types:**

1. **Threshold Monitor**: Alert when value crosses threshold
   - CPU > 90%
   - Error rate > 5%
   - Disk space < 10%

2. **Anomaly Monitor**: Alert when behavior is unusual
   - Traffic is 3x higher than normal
   - Request latency deviates from pattern

3. **Outlier Monitor**: Alert when one instance differs from others
   - One server has 50% error rate, others have 1%

4. **Forecast Monitor**: Alert before problem happens
   - Disk will be full in 2 hours at current rate

---

### **F) Synthetic Monitoring (Proactive Checks)**

**Datadog simulates user behavior to test your app**

```
Synthetic Test: "Check checkout flow"

Every 5 minutes, Datadog:
1. Visits https://yourapp.com
2. Clicks "Add to Cart"
3. Proceeds to checkout
4. Enters payment details
5. Completes purchase

If ANY step fails → Alert!

Test Results:
├─ 10:00 AM ✓ Success (2.3s)
├─ 10:05 AM ✓ Success (2.1s)
├─ 10:10 AM ✗ FAILED - Step 4 timeout
│   └─> Alert sent to OpsGenie
├─ 10:15 AM ✗ FAILED - Step 4 timeout
└─ 10:20 AM ✓ Success (2.4s) - Recovered
```

**Types of synthetic tests:**

1. **API Test**: HTTP requests to your endpoints
```
GET https://api.yourapp.com/health
Expected: Status 200, Response time < 500ms
```

2. **Browser Test**: Real browser automation
```
1. Navigate to login page
2. Enter username: test@example.com
3. Enter password: ******
4. Click "Login"
5. Assert: URL contains "/dashboard"
```

3. **Multi-step API Test**: Complex workflows
```
1. POST /api/auth/login → Get token
2. GET /api/user/profile (with token)
3. POST /api/checkout (with token)
4. Assert all steps succeed
```

---

## **Datadog Integration with AWS/K8s**

### **K8s Integration:**

```bash
# Install Datadog on K8s using Helm
helm repo add datadog https://helm.datadoghq.com
helm repo update

helm install datadog-agent datadog/datadog \
  --set datadog.apiKey=<YOUR_API_KEY> \
  --set datadog.logs.enabled=true \
  --set datadog.apm.enabled=true \
  --set datadog.processAgent.enabled=true

# Datadog agent runs as DaemonSet (one per node)
kubectl get pods -n default | grep datadog
```

**What you see in Datadog:**

```
Kubernetes Dashboard:
├─ Clusters
│  └─ production-eks
│     ├─ Nodes: 5 (all healthy)
│     ├─ Pods: 127 running, 2 pending, 0 failed
│     ├─ CPU: 45% average
│     └─ Memory: 62% average
│
├─ Services
│  ├─ frontend-service: 3 pods, 1200 req/sec
│  ├─ api-service: 5 pods, 3400 req/sec
│  └─ database-service: 2 pods, 0.2% error rate
│
└─ Events
   ├─ Pod nginx-abc123 restarted (OOMKilled)
   ├─ Node ip-10-0-1-123 added
   └─ Deployment frontend scaled to 5 replicas
```

### **AWS Integration:**

```
Datadog can monitor AWS resources:
├─ EC2 instances
├─ RDS databases
├─ Lambda functions
├─ ELB load balancers
├─ S3 buckets
├─ CloudWatch metrics
└─ AWS billing

Setup:
1. Create IAM role for Datadog
2. Grant read permissions to AWS APIs
3. Add AWS account in Datadog UI
4. Metrics start flowing automatically
```

---

## **Real-World Datadog Workflow**

**Scenario: Your API is slow**

```
1. User complains: "Checkout is slow!"

2. Check Datadog Dashboard:
   ├─ API response time: p95 = 3.2s (usually 200ms) 🚨
   └─> Something is wrong!

3. Check APM Traces:
   └─> Database queries taking 2.8s (usually 50ms)

4. Check specific trace:
   GET /api/checkout
   └─ SQL: SELECT * FROM orders WHERE user_id = ?
      ├─ Duration: 2,800ms ⚠️
      ├─ Rows returned: 1,200,000 (!!!)
      └─> Missing index on user_id column!

5. Check Logs:
   Filter: service:database level:warn
   └─> "Slow query detected: SELECT * FROM orders..."

6. Fix: Add database index
   CREATE INDEX idx_user_id ON orders(user_id);

7. Verify in Datadog:
   ├─ Response time: p95 = 180ms ✅
   └─> Issue resolved!
```

---

# **2. SITE24x7 - External Monitoring**

## **What is Site24x7?**

**Think of it as "watching your website from the customer's perspective"**

Datadog monitors FROM INSIDE your infrastructure.
Site24x7 monitors FROM OUTSIDE (like a real user).

---

## **Why You Need External Monitoring**

**Problem:**
Your internal monitoring (Datadog) says everything is fine ✅

But users can't access your website! ❌

**Why?**
- DNS issues
- CDN problems
- Network routing issues
- Firewall blocking traffic
- SSL certificate expired
- ISP-specific issues

**Site24x7 catches these issues**

---

## **Site24x7 Core Features**

### **A) Website Monitoring**

```
Site24x7 checks from 100+ global locations:

Every 1 minute (or 5 min, configurable):
├─ San Francisco, USA
├─ London, UK
├─ Tokyo, Japan
├─ Mumbai, India
└─ Sydney, Australia
    └─> All try to access https://yourapp.com

If ANY location fails → Alert!
```

**What it checks:**

```
HTTP Monitor: https://yourapp.com
├─ DNS Resolution: 45ms
├─ Connection Time: 120ms
├─ SSL Handshake: 200ms
├─ First Byte: 350ms
├─ Download Time: 180ms
├─ Total Time: 895ms
├─ HTTP Status: 200 ✓
├─ Response Size: 234 KB
└─ Content Check: Contains "Welcome" ✓

Threshold: Total time should be < 2000ms
Status: HEALTHY ✅
```

**Setting up a website monitor:**

```
Site24x7 Dashboard → Monitors → Add Monitor → Website

Configuration:
├─ URL: https://yourapp.com
├─ Check frequency: Every 1 minute
├─ Monitor locations: 10 locations worldwide
├─ Timeout: 30 seconds
├─ HTTP Method: GET
├─ Expected status code: 200
├─ Content check: Response body contains "Welcome"
├─ Alert if down from: 2+ locations
└─ Notify: OpsGenie, Email, SMS
```

---

### **B) SSL Certificate Monitoring**

```
SSL Monitor: https://yourapp.com

Certificate Details:
├─ Issuer: Let's Encrypt
├─ Valid from: 2024-01-15
├─ Valid until: 2024-04-15
├─ Days remaining: 58 days
├─ Algorithm: RSA 2048
└─ Status: VALID ✅

Alert configuration:
├─ Warn when: < 30 days remaining
└─ Critical when: < 7 days remaining
```

**Why this matters:**
If SSL cert expires → Users see "Not Secure" warning → Lost revenue

---

### **C) REST API Monitoring**

```
API Monitor: https://api.yourapp.com/health

Request:
GET /health HTTP/1.1
Host: api.yourapp.com
Authorization: Bearer xyz...

Response:
HTTP/1.1 200 OK
{
  "status": "healthy",
  "database": "connected",
  "cache": "connected",
  "latency_ms": 45
}

Checks:
├─ Status code: 200 ✓
├─ Response time: < 1000ms ✓
├─ JSON path "status": equals "healthy" ✓
├─ JSON path "latency_ms": < 100 ✓
└─> All checks passed ✅
```

**Advanced API monitoring:**

```
Multi-step API Test:

Step 1: Login
POST /api/auth/login
Body: {"username": "test", "password": "test"}
Extract: token from response

Step 2: Get User Profile
GET /api/user/profile
Header: Authorization: Bearer {{token}}
Assert: status = 200

Step 3: Create Order
POST /api/orders
Header: Authorization: Bearer {{token}}
Body: {"product": "abc", "qty": 1}
Assert: status = 201

If ANY step fails → Alert!
```

---

### **D) DNS Monitoring**

```
DNS Monitor: yourapp.com

Check from 5 DNS servers:
├─ Google DNS (8.8.8.8)
│  └─> yourapp.com → 52.12.34.56 ✓ (35ms)
├─ Cloudflare (1.1.1.1)
│  └─> yourapp.com → 52.12.34.56 ✓ (28ms)
├─ OpenDNS
│  └─> yourapp.com → 52.12.34.56 ✓ (42ms)
└─ Local ISP
   └─> yourapp.com → TIMEOUT ❌

Alert: DNS resolution failing from 1/5 servers!
```

---

### **E) Server Monitoring**

**Site24x7 can also monitor server health (like Datadog)**

```bash
# Install Site24x7 agent on server
wget https://staticdownloads.site24x7.com/server/Site24x7InstallScript.sh
bash Site24x7InstallScript.sh

# Agent monitors:
├─ CPU usage
├─ Memory usage
├─ Disk usage
├─ Network traffic
├─ Process monitoring (is nginx running?)
└─ Service monitoring (is MySQL up?)
```

---

### **F) Real User Monitoring (RUM)**

**Track actual user experience**

```html
<!-- Add JavaScript to your website -->
<script src="https://rum.site24x7.com/rum.js"></script>
<script>
  Site24x7.init({ appKey: 'your-app-key' });
</script>
```

**What it tracks:**

```
Real User Metrics (from actual users):

User: John from Mumbai
Browser: Chrome 120
Device: Desktop
Page: /checkout
├─ Page Load Time: 3.2s
├─ DNS Lookup: 120ms
├─ Connection: 450ms
├─ Backend: 1,800ms (slow!)
├─ Rendering: 830ms
└─ User Experience: Poor ⚠️

Aggregated Data:
├─ Average page load: 2.1s
├─ Slowest page: /checkout (3.2s)
├─ Fastest page: /home (800ms)
├─ Browser breakdown:
│  ├─ Chrome: 65% of users, 2.0s avg
│  ├─ Safari: 20% of users, 2.3s avg
│  └─ Firefox: 15% of users, 1.9s avg
└─ Geographic breakdown:
   ├─ US: 1.8s avg
   ├─ EU: 2.1s avg
   └─ India: 3.5s avg (slow!)
```

---

## **Site24x7 Alerting**

```
Alert Configuration:

Monitor: Website - https://yourapp.com
Condition: Down from 2+ locations
Threshold: 2 consecutive failures

Notification:
├─ Immediate: OpsGenie (pages on-call)
├─ After 5 min: Email to team
├─ After 10 min: SMS to manager
└─> Escalation policy

Alert Message:
"🚨 Website DOWN
URL: https://yourapp.com
Locations affected: Mumbai, Tokyo
Error: Connection timeout
Started: 2024-02-16 14:23:45 UTC
Duration: 8 minutes"
```

---

## **Datadog vs Site24x7: When to Use What**

| Use Case | Datadog | Site24x7 |
|----------|---------|----------|
| Internal infrastructure monitoring | ✅ | ❌ |
| Application performance (APM) | ✅ | ❌ |
| Log aggregation | ✅ | ❌ |
| External uptime monitoring | ❌ | ✅ |
| SSL certificate monitoring | ❌ | ✅ |
| Multi-location checks | ❌ | ✅ |
| Real user monitoring | ✅ | ✅ |

**In practice: Use BOTH**
- Datadog: Internal health (servers, apps, databases)
- Site24x7: External health (is website accessible?)

---

# **3. OPSGENIE - Incident Management & On-Call**

## **What is OpsGenie?**

**Think of it as "911 for your on-call team"**

When something breaks:
1. OpsGenie receives the alert
2. Determines who's on-call
3. Pages them via phone, SMS, push notification
4. Escalates if not acknowledged
5. Tracks incident until resolved

---

## **The On-Call Problem OpsGenie Solves**

**Without OpsGenie:**
```
3 AM: Production database crashes
├─> Monitoring sends email
├─> Engineer is asleep, doesn't see email
├─> 2 hours pass
├─> Customers complaining on Twitter
└─> CEO wakes up, manually calls engineers

Result: 2 hour outage, angry customers, fired engineer
```

**With OpsGenie:**
```
3 AM: Production database crashes
├─> Datadog sends alert to OpsGenie
├─> OpsGenie calls primary on-call (Jane)
├─> Jane's phone rings loudly (can't miss it)
├─> Jane acknowledges in 2 minutes
├─> Jane fixes database in 15 minutes
├─> Total downtime: 17 minutes
└─> Customers barely notice

Result: Quick resolution, happy customers, hero engineer
```

---

## **OpsGenie Core Concepts**

### **A) Teams**

```
Your Organization:
├─ SRE Team
│  ├─ Alice (Senior SRE)
│  ├─ Bob (SRE)
│  └─ Charlie (Junior SRE)
│
├─ Backend Team
│  ├─ David (Tech Lead)
│  └─ Eve (Developer)
│
└─ Frontend Team
   ├─ Frank (Tech Lead)
   └─ Grace (Developer)
```

### **B) Schedules (Who's On-Call When)**

```
SRE Team On-Call Schedule:

Week 1 (Feb 12-18):  Alice
Week 2 (Feb 19-25):  Bob
Week 3 (Feb 26-Mar 3): Charlie
Week 4 (Mar 4-10):   Alice
...repeats

Time-based rotation:
Monday-Friday 9am-5pm:  Business hours team
Monday-Friday 5pm-9am:  Night shift team
Saturday-Sunday:        Weekend team

Current on-call: Bob (Feb 20, 2024, 2:34 PM)
```

**Creating a schedule in OpsGenie:**

```
Schedule: "SRE Primary On-Call"
├─ Type: Weekly rotation
├─ Participants: Alice, Bob, Charlie
├─ Starts: Monday 9:00 AM
├─ Rotation: Every 7 days
├─ Timezone: America/New_York
├─ Restrictions:
│  ├─ Business hours only: Mon-Fri 9am-5pm
│  └─ Exclude holidays
└─ Handoff notification: 1 day before rotation
```

---

### **C) Escalation Policies**

**What if the on-call person doesn't respond?**

```
Escalation Policy: "Critical Production Alerts"

Level 1: Primary on-call (Bob)
├─> Page immediately
├─> If no acknowledgment in 5 minutes → Escalate

Level 2: Secondary on-call (Alice)
├─> Page after 5 minutes
├─> If no acknowledgment in 5 minutes → Escalate

Level 3: Manager (Tech Lead)
├─> Page after 10 minutes
├─> If no acknowledgment in 5 minutes → Escalate

Level 4: VP Engineering
└─> Page after 15 minutes (things are really bad!)

Repeat: If still no response, cycle back to Level 1
```

**Real scenario:**

```
2:47 AM: Database alert triggered
├─> OpsGenie pages Bob (primary)
├─> Bob is asleep, phone on silent (uh oh...)
├─> 5 minutes pass, no acknowledgment
├─> OpsGenie pages Alice (secondary)
├─> Alice wakes up, acknowledges in 1 minute
├─> Alice fixes the issue
└─> Bob wakes up to 12 missed calls (oops)

Post-incident: Bob learns to never silence phone on-call
```

---

### **D) Alert Routing**

**Different alerts go to different teams**

```
Routing Rules:

IF alert contains "database"
  AND priority = "critical"
  THEN route to → SRE Team

IF alert contains "payment"
  THEN route to → Backend Team

IF alert contains "UI" OR "frontend"
  THEN route to → Frontend Team

IF source = "AWS CloudWatch"
  AND service = "EC2"
  THEN route to → Infrastructure Team

Default:
  THEN route to → SRE Team (catch-all)
```

**Example:**

```
Alert: "Payment API error rate > 5%"
├─> Matches rule: contains "payment"
├─> Routed to: Backend Team
├─> Current on-call: David
└─> OpsGenie pages David
```

---

### **E) Notification Channels**

**How OpsGenie contacts you:**

```
Contact Methods (in order of urgency):

1. Push Notification (OpsGenie mobile app)
   ├─> Immediate
   └─> Can acknowledge from phone

2. SMS
   ├─> Sent after 2 minutes if not acknowledged
   └─> Text: "CRITICAL: Database down. Ack: reply ACK"

3. Phone Call
   ├─> Sent after 5 minutes if not acknowledged
   ├─> Automated voice: "Critical alert from production..."
   └─> Press 4 to acknowledge

4. Email
   ├─> Sent immediately (backup)
   └─> Least urgent

Each engineer configures their preferences:
Alice: Push → Phone (skip SMS)
Bob: Push → SMS → Phone
Charlie: Push only (junior, learns the hard way)
```

---

### **F) Integrations (How Alerts Get to OpsGenie)**

**OpsGenie integrates with 200+ tools:**

```
Alert Sources:
├─ Datadog → OpsGenie
├─ Site24x7 → OpsGenie
├─ PagerDuty → OpsGenie
├─ AWS CloudWatch → OpsGenie
├─ Prometheus → OpsGenie
├─ Slack → OpsGenie
├─ Jira → OpsGenie
└─ Custom webhooks → OpsGenie
```

**Setting up Datadog → OpsGenie:**

```
In Datadog:
1. Integrations → Search "OpsGenie"
2. Configure integration:
   ├─ OpsGenie API Key: abc123...
   └─ Save

3. In monitor configuration:
   Notify: @opsgenie-critical

In OpsGenie:
1. Settings → Integrations → Datadog
2. Copy webhook URL
3. Map Datadog priorities to OpsGenie priorities:
   ├─ Datadog P1 → OpsGenie Critical
   ├─ Datadog P2 → OpsGenie High
   └─ Datadog P3 → OpsGenie Low
```

---

### **G) Incident Timeline & Collaboration**

**OpsGenie tracks the entire incident:**

```
Incident: "Production Database Outage"
ID: INC-2024-0216-001
Status: Resolved
Duration: 23 minutes

Timeline:
├─ 02:47 AM: Alert created (Datadog: DB connection errors)
├─ 02:47 AM: Escalated to Bob (primary on-call)
├─ 02:52 AM: Escalated to Alice (Bob not responding)
├─ 02:53 AM: Alice acknowledged
├─ 02:55 AM: Alice added note: "Investigating DB logs"
├─ 03:00 AM: Alice added note: "Found issue: disk full"
├─ 03:05 AM: Alice added note: "Clearing old logs"
├─ 03:10 AM: Alert auto-resolved (DB healthy)
├─ 03:10 AM: Alice closed incident

Actions Taken:
1. Cleared 50GB of old transaction logs
2. Increased disk size from 100GB → 200GB
3. Set up alert for disk usage > 80%

Responders:
├─ Alice (Acknowledged, Resolved)
└─ Bob (Notified, Did not respond)

Postmortem: Created automatically
```

---

### **H) OpsGenie Mobile App**

**On-call engineers live in the mobile app:**

```
OpsGenie Mobile App:

┌─────────────────────────────┐
│  🔔 Alerts (3)              │
├─────────────────────────────┤
│  🚨 CRITICAL                │
│  Database connection errors  │
│  2 min ago                  │
│  [Acknowledge] [Snooze]     │
├─────────────────────────────┤
│  ⚠️  WARNING                │
│  High CPU on web-server-3   │
│  15 min ago                 │
│  [Acknowledge] [Close]      │
├─────────────────────────────┤
│  ℹ️  INFO                   │
│  Deployment completed       │
│  1 hour ago                 │
│  [View Details]             │
└─────────────────────────────┘

Bottom nav:
[Alerts] [Schedule] [Incidents] [Profile]
```

**Key features:**

- Acknowledge alerts with one tap
- Add notes/updates to incidents
- See who's on-call right now
- Take over someone's on-call shift
- View incident history
- Make conference calls with team

---

## **OpsGenie Best Practices**

### **1. Alert Fatigue Prevention**

**Bad:**
```
Every warning-level alert pages the on-call engineer
Result: 50 alerts per day, engineer ignores them all
When critical alert comes: Engineer misses it
```

**Good:**
```
Critical alerts: Page immediately
High alerts: Send to Slack, page if not handled in 30 min
Low alerts: Email only, reviewed next business day

Alert aggregation:
Instead of 20 separate "high CPU" alerts → One alert: "Multiple servers high CPU"
```

### **2. Runbooks in Alerts**

**Bad alert:**
```
"Database is down"
Engineer: "Uhh... what do I do?"
```

**Good alert:**
```
"Database is down"

Runbook:
1. Check database logs: ssh db-server, tail -f /var/log/mysql/error.log
2. Check disk space: df -h
3. If disk full: Clear old logs in /var/log/mysql/
4. Restart database: sudo systemctl restart mysql
5. If still down: Escalate to DBA team
6. Check dashboard: https://datadog.com/dash/db-health
```

### **3. On-Call Handoff**

```
When your on-call shift ends:
1. OpsGenie sends reminder 24 hours before
2. You update the next person:
   - Current incidents
   - Ongoing issues
   - Anything to watch out for
3. Next person acknowledges handoff
4. Rotation happens automatically
```

---

# **4. STATUSPAGE - Customer Communication**

## **What is Statuspage?**

**A public website that shows your service status to customers**

**Example:** https://status.yourcompany.com

```
┌─────────────────────────────────────────────────────┐
│  YourCompany Status                    [Subscribe]  │
├─────────────────────────────────────────────────────┤
│                                                      │
│  🟢 All Systems Operational                         │
│                                                      │
│  ┌────────────────────────────────────────────────┐│
│  │ Services                          Status       ││
│  ├────────────────────────────────────────────────┤│
│  │ Website                           🟢 Operational││
│  │ API                               🟢 Operational││
│  │ Mobile App                        🟢 Operational││
│  │ Payment Processing                🟢 Operational││
│  │ Email Delivery                    🟢 Operational││
│  └────────────────────────────────────────────────┘│
│                                                      │
│  Past Incidents                                      │
│  ├─ Feb 15: Slow API response times (Resolved)      │
│  ├─ Feb 10: Payment processing delays (Resolved)    │
│  └─ Feb 3: Scheduled maintenance (Completed)        │
│                                                      │
│  Uptime (Last 90 days): 99.95%                      │
│  ┌────────────────────────────────────────────────┐│
│  │ ████████████████████████████████████████████  ││
│  │ Jan        Feb                                 ││
│  └────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────┘
```

---

## **Why StatusPage Matters**

**Without StatusPage:**
```
Website is down
├─> Customers confused: "Is it just me?"
├─> Support tickets flood in: 500 tickets
├─> Twitter/Reddit explodes: "Website down!"
├─> Engineers can't focus (answering support)
└─> Bad PR

Result: Wasted time, angry customers, lost trust
```

**With StatusPage:**
```
Website is down
├─> Update StatusPage: "Investigating database issues"
├─> Customers see status: "Oh, they know about it"
├─> Fewer support tickets: 50 tickets (90% reduction)
├─> Engineers focus on fixing
├─> Regular updates posted
└─> "Resolved" posted when fixed

Result: Customer trust maintained, efficient resolution
```

---

## **StatusPage Components**

### **A) Service Status**

```
Component statuses:
🟢 Operational       - Everything working
🟡 Degraded Performance - Slower than usual
🟠 Partial Outage    - Some features broken
🔴 Major Outage      - Service completely down
🔵 Under Maintenance - Planned work
```

**Example incident:**

```
┌─────────────────────────────────────────────────────┐
│  🔴 Major Outage - Payment Processing               │
├─────────────────────────────────────────────────────┤
│  Status: Monitoring                                  │
│  Started: Feb 16, 2024 14:23 UTC                    │
│  Duration: 47 minutes                                │
│                                                      │
│  Updates:                                            │
│  ────────────────────────────────────────────────── │
│  15:10 UTC - Resolved                                │
│  Payment processing has been restored. All pending   │
│  transactions are being processed. We will continue  │
│  to monitor the system.                              │
│                                                      │
│  14:58 UTC - Monitoring                              │
│  We have implemented a fix and payments are starting │
│  to process. We are monitoring to ensure stability.  │
│                                                      │
│  14:45 UTC - Identified                              │
│  We have identified the issue with our payment       │
│  gateway provider. Our team is working on a solution.│
│                                                      │
│  14:23 UTC - Investigating                           │
│  We are currently investigating issues with payment  │
│  processing. Some customers may be unable to complete│
│  purchases. We apologize for the inconvenience.      │
│                                                      │
│  Affected Components:                                │
│  🔴 Payment Processing                               │
│  🟡 Checkout (Degraded - fallback payment enabled)   │
│                                                      │
│  Subscribers notified: 15,234 customers              │
└─────────────────────────────────────────────────────┘
```

---

### **B) Subscriber Notifications**

**Customers can subscribe to updates:**

```
Subscription options:
├─ Email (when status changes)
├─ SMS (critical incidents only)
├─ Slack integration
├─ Webhook (for their own systems)
└─ RSS feed

When incident occurs:
All subscribers get instant notification
```

**Email notification example:**

```
From: YourCompany Status <status@yourcompany.com>
Subject: [Incident] Payment Processing - Major Outage

We are currently experiencing issues with payment processing.

Status: Investigating
Started: Feb 16, 2024 14:23 UTC
Affected: Payment Processing, Checkout

We are investigating the root cause and will provide updates
every 15 minutes.

View status page: https://status.yourcompany.com

- YourCompany SRE Team
```

---

### **C) Incident Templates**

**Pre-written messages for common incidents:**

```
Template: "Database Performance Issues"

Title: Slow Response Times
Message: We are currently experiencing slower than normal response
times due to database performance issues. Our team is actively
working to resolve this. We apologize for any inconvenience.

Template: "Scheduled Maintenance"

Title: Scheduled Maintenance - Database Upgrade
Message: We will be performing scheduled maintenance on our database
servers on [DATE] from [START] to [END] [TIMEZONE]. During this time,
you may experience brief interruptions in service. We appreciate your
patience.
```

---

### **D) Automation with OpsGenie**

**Automatically update StatusPage when incidents occur:**

```
OpsGenie Alert: "Database connection errors" (Critical)
    ↓
Triggers automation
    ↓
StatusPage: Create incident
├─ Component: API
├─ Status: Major Outage
├─ Message: "We are investigating connectivity issues"
└─> Notify all subscribers

Engineer updates OpsGenie incident
    ↓
StatusPage automatically updates
    ↓
Subscribers get update email

Engineer resolves OpsGenie incident
    ↓
StatusPage marks as resolved
    ↓
Final notification sent
```

---

### **E) Metrics & Uptime**

```
Uptime tracking:

Last 90 days:
├─ API: 99.95% uptime
│  └─ 3 incidents, total downtime: 108 minutes
├─ Website: 99.98% uptime
│  └─ 1 incident, total downtime: 43 minutes
└─ Payments: 99.92% uptime
   └─ 2 incidents, total downtime: 173 minutes

Response time tracking:
┌────────────────────────────────────────────┐
│ Average API Response Time (Last 7 days)    │
│                                            │
│ 250ms ┤                                    │
│       │     ╱╲                             │
│ 200ms ┤    ╱  ╲      ╱╲                   │
│       │   ╱    ╲    ╱  ╲  ╱╲              │
│ 150ms ┤  ╱      ╲  ╱    ╲╱  ╲             │
│       │ ╱        ╲╱          ╲            │
│ 100ms ┼─────────────────────────╲─────────│
│       Mon  Tue  Wed  Thu  Fri  Sat  Sun   │
└────────────────────────────────────────────┘
```

---

## **StatusPage Best Practices**

### **1. Be Transparent**

**Bad:**
```
"We are experiencing technical difficulties"
(Vague, unhelpful)
```

**Good:**
```
"Our payment processing is currently unavailable due to an issue
with our third-party payment gateway. We are working with them
to restore service. Estimated resolution: 30 minutes."
```

### **2. Update Frequently**

```
Timeline:
14:23 - Incident starts → Post immediately: "Investigating"
14:30 - 7 min later → Update: "Identified the issue"
14:45 - 15 min later → Update: "Implementing fix"
15:00 - 15 min later → Update: "Monitoring"
15:10 - 10 min later → Final: "Resolved"

Don't go silent for hours!
```

### **3. Scheduled Maintenance**

```
Post scheduled maintenance 7 days in advance:

Feb 9: "Scheduled Maintenance on Feb 16"
Feb 14: "Reminder: Maintenance in 2 days"
Feb 16: "Maintenance starting now"
Feb 16 (done): "Maintenance completed"
```

---

# **5. CLOUDHEALTH - Cloud Cost Optimization**

## **What is CloudHealth?**

**Think of it as "your cloud expense accountant + financial advisor"**

Monitors AWS/Azure/GCP spending and suggests ways to save money.

---

## **The Cloud Cost Problem**

**Without CloudHealth:**
```
Month 1: AWS bill = $5,000 (seems reasonable)
Month 2: AWS bill = $8,000 (hmm, growing)
Month 3: AWS bill = $15,000 (WTF?!)
Month 4: AWS bill = $25,000 (CEO is FURIOUS)

Engineer: "I don't know where the money is going!"
```

**With CloudHealth:**
```
CloudHealth Dashboard:

Monthly spend: $15,000
Breakdown:
├─ EC2: $8,000 (53%)
│  ├─ Production: $5,000
│  └─ Dev/Test: $3,000 ⚠️ (left running 24/7!)
│
├─ RDS: $4,000 (27%)
│  └─ Old database (unused): $1,200 ⚠️
│
├─ S3: $2,000 (13%)
│  └─ Glacier transition savings: $500/month
│
└─ Data Transfer: $1,000 (7%)

Recommendations:
1. Stop dev/test instances at night → Save $1,800/month
2. Delete unused database → Save $1,200/month
3. Use Reserved Instances → Save $2,000/month
Total potential savings: $5,000/month (33%!)
```

---

## **CloudHealth Core Features**

### **A) Cost Visibility**

```
Cost breakdown by:
├─ Service (EC2, RDS, S3, etc.)
├─ Environment (Production, Staging, Dev)
├─ Team (SRE, Backend, Frontend)
├─ Project (Customer Portal, API, Mobile App)
└─ Tag (cost-center: engineering)

Example:
┌────────────────────────────────────────────┐
│ February 2024 Spending                     │
├────────────────────────────────────────────┤
│ EC2:             $8,000  ████████████████  │
│ RDS:             $4,000  ████████          │
│ S3:              $2,000  ████              │
│ Data Transfer:   $1,000  ██                │
│ Lambda:            $500  █                 │
│ CloudWatch:        $300  ▌                 │
│ Other:             $200  ▌                 │
├────────────────────────────────────────────┤
│ Total:          $16,000                    │
│ vs Last Month:  +$1,500 (+10%) ⚠️          │
└────────────────────────────────────────────┘
```

---

### **B) Cost Anomaly Detection**

```
CloudHealth Alert: Cost Anomaly Detected!

Service: EC2
Region: us-east-1
Date: Feb 15, 2024

Normal daily cost: $200
Today's cost: $1,200 ⚠️ (+500%)

Root cause:
├─ Instance type: m5.24xlarge (96 vCPUs)
├─ Launched: Feb 15, 02:34 AM
├─ Tag: created-by: john@company.com
└─> John accidentally launched huge instance!

Action: Auto-alert sent to John and manager
Recommendation: Terminate instance to avoid further charges
```

---

### **C) Reserved Instance Recommendations**

**Understanding Reserved Instances:**

```
On-Demand pricing: $0.10/hour
Reserved Instance (1-year): $0.06/hour (40% savings)
Reserved Instance (3-year): $0.04/hour (60% savings)

If you run an instance 24/7:
├─ On-Demand cost: $876/year
├─ Reserved (1-year): $526/year → Save $350
└─ Reserved (3-year): $350/year → Save $526
```

**CloudHealth analysis:**

```
Reserved Instance Recommendations:

Instance: t3.medium in us-east-1
Usage: Running 24/7 for 6 months
Current cost (On-Demand): $365/year

Recommendation: Purchase 1-year Reserved Instance
├─ Cost: $219/year
├─ Savings: $146/year (40%)
└─> Break-even: 7.3 months (you've already run 6 months!)

Total RI opportunities: 23 instances
Potential annual savings: $12,500
```

---

### **D) Rightsizing Recommendations**

```
Underutilized Resources:

Instance: web-server-5 (m5.large - 2 vCPU, 8GB RAM)
Actual usage:
├─ Average CPU: 8%
├─ Average Memory: 2GB (25%)
└─> Over-provisioned!

Recommendation: Downsize to t3.medium (2 vCPU, 4GB RAM)
├─ Current cost: $70/month
├─ New cost: $30/month
└─> Savings: $40/month ($480/year)

Overutilized Resources:

Instance: api-server-2 (t3.medium)
Actual usage:
├─ Average CPU: 95% (maxed out!)
├─> Performance bottleneck

Recommendation: Upsize to t3.large
├─> Better performance
└─> Cost increase: $30/month (worth it for performance)
```

---

### **E) Idle Resource Detection**

```
Idle Resources (Wasting Money):

EC2 Instances:
├─ test-server-old (running 30 days, 0% CPU)
│  └─> Cost: $72/month (WASTE)
├─ staging-db (RDS, no connections in 14 days)
│  └─> Cost: $120/month (WASTE)
└─ dev-instance-john (stopped, but EBS volume attached)
   └─> Cost: $20/month

Total waste: $212/month

Recommendations:
1. Terminate test-server-old
2. Delete snapshot and terminate staging-db
3. Delete unused EBS volumes
Monthly savings: $212
```

---

### **F) Budget Alerts**

```
Budget: "Production Environment"
Limit: $10,000/month

Current spend: $8,500 (85% of budget)

Alert thresholds:
├─ 50%: $5,000 → Email notification (Feb 8)
├─ 75%: $7,500 → Email + Slack (Feb 14)
├─ 90%: $9,000 → Email + Slack + Manager (Feb 18)
└─ 100%: $10,000 → Escalate to VP Engineering

Forecast:
Based on current trend, you will exceed budget by:
├─ End of month spend: $11,200
└─> Overage: $1,200 ⚠️

Action required: Reduce spending or request budget increase
```

---

### **G) Showback/Chargeback**

**Show teams their cloud costs:**

```
Monthly Cost Report by Team:

Backend Team:
├─ EC2: $3,000
├─ RDS: $2,000
├─ S3: $500
└─ Total: $5,500

Frontend Team:
├─ EC2: $1,500
├─ CloudFront: $800
├─ S3: $300
└─> Total: $2,600

SRE Team:
├─ Monitoring (Datadog): $1,200
├─ Logging: $800
├─ Infrastructure: $2,000
└─> Total: $4,000

This creates accountability!
Teams become cost-conscious when they see their own spending.
```

---

## **CloudHealth Best Practices**

### **1. Tag Everything**

```
Proper tagging strategy:

Every resource should have tags:
├─ Environment: production | staging | dev
├─ Team: backend | frontend | sre
├─ Project: customer-portal | mobile-api
├─ Cost-Center: engineering | sales
└─ Owner: john@company.com

Example EC2 instance:
Name: api-server-prod-1
Tags:
  Environment: production
  Team: backend
  Project: customer-api
  Cost-Center: engineering
  Owner: alice@company.com

This enables accurate cost allocation!
```

### **2. Automate Cost Optimization**

```
Automation policies:

1. Stop non-production instances at night
   ├─ Tag: Environment != production
   ├─ Schedule: Stop at 6 PM, Start at 8 AM weekdays
   └─> Savings: 60% of compute cost

2. Delete old snapshots
   ├─ Condition: Older than 30 days
   └─> Free up storage costs

3. Transition S3 to Glacier
   ├─ Objects older than 90 days
   └─> 80% storage cost reduction
```

---

# **How These Tools Work Together**

## **The Complete SRE Workflow**

```
┌─────────────────────────────────────────────────────────────┐
│                    MONITORING LAYER                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────┐  ┌───────────┐  ┌──────────────┐           │
│  │ DATADOG  │  │ SITE24x7  │  │ CLOUDHEALTH  │           │
│  │(Internal)│  │(External) │  │  (Costs)     │           │
│  └─────┬────┘  └─────┬─────┘  └──────┬───────┘           │
│        │             │                 │                    │
│        │ Metrics     │ Uptime checks   │ Cost anomalies    │
│        │ Logs        │ SSL expiry      │ Budget alerts     │
│        │ Traces      │ API tests       │                    │
│        │             │                 │                    │
└────────┼─────────────┼─────────────────┼────────────────────┘
         │             │                 │
         │             │                 │
         ▼             ▼                 ▼
    ┌────────────────────────────────────────┐
    │           OPSGENIE                     │
    │      (Incident Management)             │
    │                                        │
    │  Receives alerts from all sources      │
    │  Routes to on-call engineer            │
    │  Escalates if no response              │
    │  Tracks incident lifecycle             │
    └────────────┬───────────────────────────┘
                 │
                 │ Pages on-call
                 │ Updates incident
                 │
                 ▼
    ┌────────────────────────────────────────┐
    │         ON-CALL ENGINEER               │
    │                                        │
    │  1. Gets paged by OpsGenie             │
    │  2. Acknowledges alert                 │
    │  3. Checks Datadog for details         │
    │  4. Debugs using logs/traces/metrics   │
    │  5. Fixes the issue                    │
    │  6. Updates StatusPage                 │
    │  7. Resolves incident in OpsGenie      │
    └────────────┬───────────────────────────┘
                 │
                 │ Updates
                 │
                 ▼
    ┌────────────────────────────────────────┐
    │          STATUSPAGE                    │
    │    (Customer Communication)            │
    │                                        │
    │  Shows current status to customers     │
    │  Posts incident updates                │
    │  Notifies subscribers                  │
    └────────────────────────────────────────┘
```

---

## **Real Incident Example: Putting It All Together**

```
3:42 AM: Database performance degrades

DATADOG:
├─ Detects: Database query latency > 2000ms (threshold: 500ms)
├─> Triggers monitor: "Database Slow Queries"
└─> Sends alert to OpsGenie

SITE24x7:
├─> External checks start failing: API timeout
└─> Also sends alert to OpsGenie

OPSGENIE:
├─> Receives 2 alerts (Datadog + Site24x7)
├─> Correlates: Same incident
├─> Pages primary on-call: Alice
└─> Alice's phone: LOUD RINGING

3:44 AM: Alice acknowledges

ALICE's WORKFLOW:
1. Opens OpsGenie app: "Database slow + API timeout"
2. Opens Datadog on laptop:
   ├─> APM traces show 95% of time in database
   ├─> Logs show: "Slow query: SELECT * FROM orders..."
   └─> Identifies: Missing database index!

3:50 AM: Alice updates StatusPage

STATUSPAGE:
├─> Post: "Investigating slow response times"
├─> Component: API (Degraded Performance)
└─> 5,234 subscribers notified via email

3:52 AM: Alice fixes the issue
├─> Adds database index: CREATE INDEX idx_user_id ON orders(user_id)
└─> Query time: 2000ms → 50ms ✅

3:55 AM: Systems recover

DATADOG:
└─> Monitor auto-resolves (latency back to normal)

SITE24x7:
└─> External checks passing again

OPSGENIE:
├─> Auto-detects recovery
└─> Alice marks incident as resolved

STATUSPAGE:
├─> Alice posts: "Resolved - Database performance restored"
└─> Subscribers notified

4:00 AM: Post-incident

OPSGENIE:
└─> Generates incident report automatically

CLOUDHEALTH:
└─> Notes spike in RDS CPU during incident (for cost tracking)

ALICE:
├─> Writes postmortem runbook
├─> Goes back to sleep
└─> Hero of the day ✅

Total downtime: 18 minutes
Customer impact: Minimal (transparent communication)
Root cause: Missing database index
Prevention: Added monitoring for slow queries
```

---

## **Your Learning Roadmap**

### **Week 1-2: Datadog**
```
□ Install Datadog agent on a test server
□ Send custom metrics
□ Set up log collection
□ Create your first dashboard
□ Configure a monitor with alerting
□ Instrument a simple app for APM
□ Practice: Deploy an app, break it, debug with Datadog
```

### **Week 3: Site24x7**
```
□ Sign up for Site24x7 trial
□ Create website monitor
□ Set up SSL certificate monitoring
□ Create API monitor
□ Configure alert notifications
□ Practice: Monitor your personal website/project
```

### **Week 4: OpsGenie**
```
□ Sign up for OpsGenie trial
□ Create a team
□ Set up on-call schedule
□ Create escalation policy
□ Integrate with Datadog
□ Practice: Trigger test alert, acknowledge on mobile app
```

### **Week 5: StatusPage**
```
□ Sign up for StatusPage trial
□ Create status page
□ Add components (API, Website, Database)
□ Create test incident
□ Practice updating incident
□ Integrate with OpsGenie
□ Subscribe via email to see notifications
```

### **Week 6: CloudHealth**
```
□ Sign up for CloudHealth trial (need AWS account)
□ Connect AWS account
□ Review cost dashboard
□ Check rightsizing recommendations
□ Set up budget alert
□ Practice: Tag resources, view cost allocation
```

---

## **Hands-On Practice Projects**

### **Project 1: Deploy & Monitor**
```
1. Deploy a simple web app (Node.js/Python)
2. Instrument with Datadog APM
3. Create Datadog dashboard
4. Set up Site24x7 external monitoring
5. Configure OpsGenie alerting
6. Create StatusPage
7. Break the app intentionally
8. Practice incident response workflow
```

### **Project 2: Simulate Incident**
```
Scenario: Database becomes slow

1. Slow down database queries artificially
2. Datadog detects high latency
3. Alert fires to OpsGenie
4. You get paged
5. Update StatusPage
6. Debug using Datadog APM/logs
7. Fix the issue
8. Update StatusPage (resolved)
9. Write postmortem
```

---

## **Key Takeaways**

**Remember:**

1. **Datadog** = Eyes inside your infrastructure (metrics, logs, traces)
2. **Site24x7** = Eyes from outside (uptime, SSL, global checks)
3. **OpsGenie** = Ensures alerts reach humans (on-call, escalation)
4. **StatusPage** = Keeps customers informed (transparency)
5. **CloudHealth** = Keeps costs under control (optimization)

**They work together:**
- Monitoring tools detect issues
- OpsGenie alerts the right person
- Engineer fixes using monitoring data
- StatusPage keeps customers informed
- CloudHealth ensures you're not overspending

---

**You're now equipped with the knowledge of these tools. Start practicing with free trials of each, and by the time you join your SRE role, you'll be ahead of most new hires!**

**Any specific tool you want me to dive deeper into? Or ready to start practicing?**
