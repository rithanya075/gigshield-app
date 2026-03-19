# GigShield
An AI-powered weekly parametric insurance solution for platform-based food delivery workers in India, featuring dynamic risk-based premium pricing, automated claim triggers, real-time fraud detection, and instant UPI payouts. 


> *When the rain stops a Swiggy rider from working, GigShield makes sure it doesn't stop him/her from eating.*

---

##  The Problem Statement

India has over 5 million platform-based food delivery partners working for Zomato and Swiggy. These riders are the backbone of our digital food economy — yet they have **zero financial protection** against uncontrollable external disruptions.

**Real scenario:**
> Ravi is a 28-year-old Swiggy delivery partner in Chennai. During monsoon season, heavy rain forces him to stay home 3–4 days per month. Each lost day costs him ₹600–₹800. That's ₹2,000–₹3,200 lost every monsoon month — money he needed for rent, fuel, and food for his family. No insurance covers this. No platform compensates him. He bears the full loss alone.

**When disruptions hit, riders lose 20–30% of their monthly income with no safety net.**

External disruptions include:
- Heavy rainfall making roads dangerous and orders unavailable
- Extreme heat (42°C+) making outdoor work unsafe
- Severe air pollution (AQI 300+) making delivery impossible
- Local curfews or strikes blocking access to pickup/drop zones
- Platform outages causing zero order availability

---

##  Our Solution — GigShield

GigShield is a **weekly parametric income insurance platform** built exclusively for Zomato and Swiggy delivery partners.

The rider pays a small weekly premium. When an external disruption is detected in their delivery zone, **GigShield automatically detects it, approves the claim, and sends the payout to their UPI account — with zero forms, zero calls, and zero waiting.**

### How It Works in 4 Steps
```
1. Rider buys a weekly policy (₹29 / ₹59 / ₹99)
        ↓
2. Rider works normally — GPS activity tracked passively
        ↓
3. Disruption detected automatically (rain / heat / curfew)
        ↓
4. Claim auto-approved → Money in UPI within seconds
```

---

## 👤 Persona & Scenarios

### Persona: Food Delivery Partner (Zomato / Swiggy)

| Attribute | Detail |
|---|---|
| Age | 22–40 years |
| City | Chennai, Mumbai, Bengaluru, Hyderabad, Delhi |
| Daily Earnings | ₹600 – ₹1,000 |
| Weekly Earnings | ₹3,500 – ₹6,000 |
| Working Hours | 8–12 hours/day |
| Platform | Zomato and/or Swiggy |
| Risk | Loses income during rain, heat, curfew, platform outages |

### Scenario 1 — Heavy Rain
> It's a Tuesday afternoon in Mumbai. Rainfall hits 45mm in Andheri zone. Zomato orders drop to near zero. Ravi can't work. GigShield detects the rainfall threshold breach via OpenWeatherMap API, auto-triggers a claim for Ravi, and credits ₹420 to his UPI account for 6 lost hours. Ravi never opened the app.

### Scenario 2 — Extreme Heat
> It's May in Delhi. Temperature hits 44°C at 1pm. The city issues a heat advisory. GigShield detects the temperature threshold breach, checks that Ravi has an active Pro policy, and automatically pays him ₹350 for 5 lost afternoon hours.

### Scenario 3 — Local Curfew
> A sudden Section 144 curfew is imposed in Bengaluru's Koramangala zone. GigShield's curfew monitor detects the event via news/government API, validates that Ravi's registered zone is affected, and triggers an automatic payout of ₹500.

### Scenario 4 — Fraud Attempt (Blocked)
> Ramesh tries to fake a rain claim by reporting disruption in his zone. GigShield's fraud engine checks: (1) his GPS shows he was actively moving during the claimed disruption period, (2) his earning pattern shows he normally earns even on light rain days, (3) the rainfall in his zone was only 8mm — below threshold. Claim rejected automatically.

---

##  Weekly Premium Model

Gig workers earn week-to-week. Our pricing is structured on a **weekly basis** to match their natural earnings cycle.

### Base Pricing Tiers

| Plan | Weekly Premium | Max Coverage | Best For |
|---|---|---|---|
|  Basic | ₹29 | ₹300/week | Riders in low-risk zones |
|  Standard | ₹59 | ₹700/week | Most riders |
|  Pro | ₹99 | ₹1,200/week | Riders in high-risk zones |

### AI-Adjusted Dynamic Pricing

The base price is **dynamically adjusted every Monday** by our AI Risk Engine based on:

- **Zone Risk Score (1–10):** Historical flood/heat frequency in the rider's delivery zone
- **Seasonal Factor:** Monsoon months increase risk score by 1.5x
- **Personal History:** Riders with zero fraud flags get a 5% loyalty discount
- **Forecast Factor:** If next week's weather forecast shows 70%+ rain probability, coverage window increases automatically

**Example:**
> Standard plan base = ₹59. Ravi operates in Chennai's Tambaram zone (Risk Score: 8/10, monsoon season). AI adjusts his premium to ₹71 for that week with extended coverage hours.

### Policy Rules
- Policy activates every **Monday at 00:00**
- Policy expires every **Sunday at 23:59**
- Rider can upgrade tier mid-week if bad weather is predicted
- Maximum **2 claims per week** per rider
- Payout = (Lost hours × Hourly rate) capped at weekly coverage limit

---

##  Parametric Triggers

GigShield monitors **5 automated triggers** using real-time and mock APIs. No manual claim filing — ever.

| # | Trigger | Threshold | Data Source | Payout Basis |
|---|---|---|---|---|
| 1 | Heavy Rainfall | > 30mm in 3 hours | OpenWeatherMap API | Per lost hour |
| 2 | Extreme Heat | > 42°C for 2+ hours | OpenWeatherMap API | Per lost hour |
| 3 | Severe Air Pollution | AQI > 300 | OpenAQ API (free) | Per lost hour |
| 4 | Local Curfew / Strike | Zone flagged | Mock Government API | Flat daily rate |
| 5 | Platform Outage | Swiggy/Zomato down > 2hrs | Mock Platform API | Per lost hour |

### How a Trigger Works (Step by Step)
```
Every 30 minutes:
  → Trigger Engine polls weather + event APIs
  → Checks all active policies in affected zones
  → If threshold breached:
      → Creates claim record
      → Sends to Fraud Detector
      → If fraud check passes → Auto-approve
      → Sends payout via Razorpay Sandbox
      → Pushes notification to rider app
  → Total time from trigger to payout: < 60 seconds
```

---

## 🤖 AI / ML Integration Plan

### 1. Zone Risk Scoring Model
- **Algorithm:** Gradient Boosted Decision Tree (XGBoost)
- **Input Features:** Historical rainfall data, flood frequency, heat index, zone geography, past claim rates
- **Output:** Risk score 1–10 per zone, updated weekly
- **Used for:** Dynamic premium calculation every Monday

### 2. Earning Fingerprint — Fraud Detection
- **Algorithm:** Isolation Forest (anomaly detection)
- **Input Features:** Historical daily earnings per rider, time of day, day of week, weather on that day, GPS activity
- **Output:** Anomaly score for each incoming claim
- **Used for:** Flagging claims where earning loss doesn't match rider's historical pattern

### 3. Disruption Forecast Nudge
- **Algorithm:** Time series forecasting (Facebook Prophet)
- **Input:** 7-day weather forecast from OpenWeatherMap
- **Output:** Probability of disruption next week per zone
- **Used for:** Sending push notifications to uninsured riders before storms

### 4. GPS Activity Validator
- **Logic:** Rule-based cross-check
- **Input:** Rider GPS pings during claimed disruption window
- **Logic:** If rider moved > 2km during disruption window → flag as suspicious
- **Used for:** Real-time fraud prevention during claim processing

---

## 🛠️ Tech Stack

### Frontend
| Layer | Technology | Purpose |
|---|---|---|
| Mobile App | React Native | Rider-facing app (iOS + Android) |
| Web Dashboard | React.js + Tailwind CSS | Insurer admin dashboard |

### Backend
| Layer | Technology | Purpose |
|---|---|---|
| API Server | Node.js + Express | Core REST APIs |
| AI Engine | Python + FastAPI | ML models + risk scoring |
| Database | PostgreSQL | Riders, policies, claims |
| Cache | Redis | Real-time trigger state |
| Job Scheduler | node-cron | Trigger polling every 30 mins |

### Integrations
| Service | Provider | Mode |
|---|---|---|
| Weather Data | OpenWeatherMap | Free tier (live) |
| Air Quality | OpenAQ | Free tier (live) |
| Payment Gateway | Razorpay | Test/Sandbox mode |
| Curfew / Strike | Custom simulator | Mock API |
| Platform Activity | Custom simulator | Mock API |

### Infrastructure
| Service | Provider |
|---|---|
| Frontend hosting | Vercel |
| Backend hosting | Render |
| Database | Supabase (PostgreSQL) |
| Mobile builds | Expo (React Native) |

---

##  Repository Structure
```
gigshield/
│
├── README.md
│
├── frontend/
│   ├── mobile/                   ← React Native rider app
│   │   ├── screens/
│   │   │   ├── Onboarding.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Policy.jsx
│   │   │   └── Payouts.jsx
│   │   └── App.jsx
│   │
│   └── web/                      ← Admin dashboard
│       ├── pages/
│       │   ├── Dashboard.jsx
│       │   ├── Claims.jsx
│       │   ├── FraudAlerts.jsx
│       │   └── Analytics.jsx
│       └── App.jsx
│
├── backend/
│   ├── src/
│   │   ├── routes/
│   │   │   ├── riders.js
│   │   │   ├── policies.js
│   │   │   ├── claims.js
│   │   │   └── payouts.js
│   │   ├── services/
│   │   │   ├── triggerEngine.js
│   │   │   ├── fraudDetector.js
│   │   │   └── payoutService.js
│   │   ├── models/
│   │   │   ├── Rider.js
│   │   │   ├── Policy.js
│   │   │   └── Claim.js
│   │   └── index.js
│   └── package.json
│
├── ai-engine/
│   ├── premium_calculator.py     ← Zone risk + weekly pricing model
│   ├── fraud_model.py            ← Isolation forest anomaly detection
│   ├── forecast_nudge.py         ← Disruption probability forecasting
│   └── requirements.txt
│
├── mock-apis/
│   ├── swiggy_simulator.js       ← Fake platform earnings + activity
│   └── curfew_simulator.js       ← Fake curfew and strike events
│
└── docs/
    ├── architecture.png
    └── pitch-deck.pdf
```

---

## 🗓️ Development Plan

### Phase 1 — Ideation & Foundation (Week 1–2) 
- [x] Problem research and persona definition
- [x] Weekly premium model design
- [x] Parametric trigger identification
- [x] AI/ML architecture planning
- [x] Tech stack finalization
- [x] Repository setup and README

### Phase 2 — Automation & Protection (Week 3–4)
- [ ] Rider registration and onboarding flow
- [ ] Zone risk scoring model (Python)
- [ ] Weekly policy creation with dynamic pricing
- [ ] 5 parametric trigger implementations
- [ ] Basic fraud detection (GPS + pattern check)
- [ ] Claims management system
- [ ] Razorpay sandbox payout integration

### Phase 3 — Scale & Optimise (Week 5–6)
- [ ] Advanced fraud detection (Isolation Forest model)
- [ ] Disruption forecast nudge system
- [ ] Rider dashboard (earnings protected, payout history)
- [ ] Insurer command center (live map, loss ratio, predictions)
- [ ] End-to-end demo with simulated rainstorm trigger
- [ ] Final pitch deck

---

##  Innovation Highlights

Beyond the must-have features, GigShield introduces:

###  Weather Twin Zone Intelligence
Every delivery zone in India gets a historical "weather twin" profile. Zones are scored 1–10 based on 5 years of rainfall, flood, and heat data. This makes premium pricing hyper-local and genuinely fair — a rider in Bengaluru's flood-prone Ejipura pays more than a rider in dry Whitefield.

### Earning Fingerprint Fraud Detection
Every rider builds a unique earning fingerprint over time — how much they typically earn on rainy Tuesdays, hot May afternoons, or festive Sundays. Any claim that deviates significantly from this fingerprint is automatically flagged before payout.

###  Disruption Forecast Nudge
48 hours before a predicted disruption, uninsured riders in the affected zone receive a push notification: *"Heavy rain predicted in your zone Thursday. Get covered for ₹59 this week."* This solves rider churn and drives weekly renewals proactively.

###  Streak Loyalty Reward
Riders who renew for 4 consecutive weeks get their 5th week free. Riders with zero fraud flags for 3 months get a permanent 5% discount. This builds long-term retention and rewards honest riders.

---

##  Coverage Exclusions (Golden Rules)

GigShield strictly covers **loss of income ONLY.**

The following are explicitly **NOT covered:**
-  Vehicle repair or damage
-  Health expenses or medical bills
-  Accident compensation
-  Life insurance
-  Any personal asset damage

---

## Team Name : MIND FORGE
TEAM MEMBERS :
RITHANYA S, OVIYA SHRI V,
JANAKI RAMAN A, JOHN MEDWIN G

---

##  License

MIT License — Built for Guidewire DEVTrails 2026

---

*GigShield — Protecting India's delivery heroes, one week at a time.* 
