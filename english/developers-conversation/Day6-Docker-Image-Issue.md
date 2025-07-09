# 👨‍💻 Day 6 – Discussing Docker Image Issue with Developer

## 🎯 Scenario
A deployment failed due to a wrong or broken Docker image. You’re debugging it with the developer — it may be a missing tag, corrupted build, or cache issue.

---

## 🗣️ Sample Message

Hey, the service is failing to start — I checked and it’s pulling the `latest` image, but the digest has changed.  
Could you confirm if a new build was pushed recently without changing the tag?  
Using mutable tags like `latest` might be risky — I suggest switching to versioned tags going forward.  
Let me know once the correct image is pushed — I’ll redeploy and verify.

---

## ✅ 5 Useful Phrases

- The image tag `latest` pulled a different version than expected.
- It might be a broken or partial image upload.
- Can you push the build again with a proper version tag?
- We should avoid mutable tags in production.
- I’ll redeploy once you confirm the image is fixed.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **Image digest** | Unique hash of a Docker image |
| **Tag** | Label used to identify a specific image version |
| **Mutable tag** | A tag like `latest` that can point to different builds |
| **Corrupted image** | An incomplete or broken Docker build |
| **Redeploy** | Triggering a new deployment with a fixed image |
