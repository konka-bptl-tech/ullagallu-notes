# 🧑‍💼 Day 6 – Reporting Incident Root Cause to Manager

## 🎯 Scenario
A service outage occurred. You’re sharing the RCA (Root Cause Analysis) with your manager — what happened, what caused it, how it was fixed, and how you’ll prevent it.

---

## 🗣️ Sample Message

Hi, I’ve completed the root cause analysis for yesterday’s incident.  
The outage was caused by a faulty container image pushed without health check validation.  
The ECS service couldn’t stabilize and went into a restart loop.  
We’ve rolled back to the stable image and added pre-deploy checks in the pipeline to catch such issues early.  
I'll document the findings and share the post-incident report shortly.

---

## ✅ 5 Useful Phrases

- The root cause was a faulty image without proper validation.
- The service entered a crash loop and failed to recover.
- We rolled back to the previous stable image.
- A new pre-deployment check has been added to the CI/CD pipeline.
- I’ll share a detailed incident report and prevention steps.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **Root Cause Analysis (RCA)** | Identifying the actual reason for an incident |
| **Crash loop** | Continuous restarting due to failure |
| **Rollback** | Reverting to a previous working version |
| **Pre-deploy check** | Validation before code/image is deployed |
| **Post-incident report** | A formal write-up of the issue and fix |
