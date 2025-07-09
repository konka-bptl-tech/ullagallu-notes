# 👨‍💻 Day 7 – Collaborating on API Rate Limit / Throttling Issue

## 🎯 Scenario  
Your app is hitting API rate limits and failing intermittently. You’re coordinating with the developer to adjust call frequency or add retry logic.

---

## 🗣️ Sample Message

Hey, I noticed some 429 errors in our backend logs — looks like we’re hitting the third-party API rate limit.  
Can we check if the retry mechanism is working properly, or if we’re making too many requests in a short time?  
Maybe we can batch some of the requests or increase the delay between retries.  
Let me know if you need help updating the code or testing the fix in staging.

---

## ✅ 5 Useful Phrases

- The logs show repeated 429 errors from the external API.
- We’re exceeding the allowed request rate.
- Can we implement exponential backoff for retries?
- Let’s add proper error handling to prevent app crashes.
- We should test this with load simulation before deploying to prod.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **429 error** | Too many requests — server is throttling |
| **Rate limiting** | Restricting number of requests per time window |
| **Retry logic** | Code that retries failed requests |
| **Exponential backoff** | Increasing delay after each failed attempt |
| **Batching** | Combining multiple API calls into one request |
