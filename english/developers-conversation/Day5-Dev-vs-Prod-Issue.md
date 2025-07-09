# 👨‍💻 Day 5 – Debugging Environment-Specific Issue with Developer

## 🎯 Scenario
The app works fine in the development environment but fails in production. You’re discussing possible root causes with the developer.

---

## 🗣️ Sample Message

Hey, the feature is working in dev but throwing a 500 error in production.  
I compared the environment variables — looks like the API key is different in prod.  
Can you double-check the config? I’ll redeploy once we confirm the correct value.  
Also, let’s consider syncing both environments to avoid drift.

---

## ✅ 5 Useful Phrases

- It’s working in dev but failing in production.
- I suspect a config or environment mismatch.
- The logs show a 500 error — maybe from a missing or wrong secret.
- Let’s validate the environment-specific values.
- We should sync dev and prod configs to prevent future drift.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **500 error** | Server error, usually from unhandled issues |
| **Environment variable** | Configuration values for different environments |
| **Drift** | Differences between dev, staging, and prod setups |
| **Config** | Application or infrastructure settings |
| **Redeploy** | Deploying the service again with updated settings |
