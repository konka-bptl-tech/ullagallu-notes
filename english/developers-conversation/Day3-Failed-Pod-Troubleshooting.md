# 👨‍💻 Day 3 – Troubleshooting Failed Pod with Developer

## 🎯 Scenario
A pod in EKS keeps restarting. You’re working with the developer to identify if it's due to a code crash or resource limit.

---

## 🗣️ Sample Message

Hey, I noticed the pod for the backend is in a CrashLoopBackOff state.  
I checked the logs — it shows a panic error on startup, likely from the application.  
Can you verify if the app has any missing config or uncaught error?  
Meanwhile, I’ll increase the memory limits to see if that helps stabilize the pod.

---

## ✅ 5 Useful Phrases

- The pod is in a CrashLoopBackOff state.
- Logs show the container exits immediately after starting.
- It could be a code crash or an unhandled exception.
- I’m increasing the memory limit temporarily.
- Let’s test a fix in staging before pushing to prod.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **CrashLoopBackOff** | Pod keeps crashing and restarting |
| **Pod logs** | Output showing app behavior inside a pod |
| **Memory limit** | Max memory a pod can use |
| **Unhandled exception** | Code crash not caught by error handling |
| **Staging** | Pre-production environment for testing |
