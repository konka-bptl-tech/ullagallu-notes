# 👨‍💻 Day 1 – CI/CD Issue Debugging with Developer

## 🎯 Scenario
A deployment failed in the CI/CD pipeline. You're discussing with the developer to identify the root cause.

---

## 🗣️ Sample Message

Hey, I noticed the deployment failed in GitHub Actions.  
The build step is throwing an error due to a missing environment variable.  
Can you confirm if the `.env` file or secret was updated in the repo?  
Once you fix that, I can rerun the pipeline.

---

## ✅ 5 Useful Phrases

- The deployment failed at the build/test step.
- Looks like an environment variable is missing.
- Can you check if the latest commit includes the secret?
- I paused the pipeline until we verify the fix.
- Let me know once it's corrected — I’ll trigger the job again.

---

## 🧠 Word Bank (Industry Terms)

| Term | Meaning |
|------|---------|
| **CI/CD** | Continuous Integration and Deployment |
| **Pipeline** | Series of automated steps to test and deploy code |
| **Environment variable** | Configuration values required by the app |
| **Build step** | The stage where code is compiled or containerized |
| **GitHub Actions** | Tool used to automate the pipeline |
