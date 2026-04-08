Day 6 is where you level up from “user” → “DevOps engineer”

Anyone can deploy.
👉 Real engineers debug broken systems.

🧠 DAY 6 — WHY PODS FAIL
🔥 Reality

Pods fail ALL THE TIME in real systems.

👉 Your job:

Find problem → Fix fast

🚨 COMMON FAILURE TYPES (VERY IMPORTANT)
❌ 1. ImagePullBackOff

👉 Cause:

Wrong image name
Private repo without login
❌ 2. CrashLoopBackOff

👉 Cause:

App crashing
Wrong env variables
Code error
❌ 3. Pending

👉 Cause:

Not enough resources
Scheduling issue
❌ 4. ErrImagePull

👉 Image not found

🧠 DEBUGGING FLOW (GOLD 🔥)

Always follow this:

1. kubectl get pods
2. kubectl describe pod
3. kubectl logs pod

👉 NEVER guess blindly

🚀 HANDS-ON TASK (BREAK & FIX)
💣 STEP 1 — BREAK YOUR APP

Edit your deployment:

env:
  - name: PORT
    value: "wrong"

OR:

image: wrong-image-name
Apply:
kubectl apply -f deployment.yaml
🔍 STEP 2 — CHECK STATUS
kubectl get pods

👉 Example:

CrashLoopBackOff ❌
🔎 STEP 3 — DESCRIBE POD
kubectl describe pod <pod-name>

👉 Look for:

Events
Errors
📜 STEP 4 — CHECK LOGS (MOST IMPORTANT)
kubectl logs <pod-name>

👉 Example:

Error: PORT must be number
🧠 THIS IS THE KEY

👉 Logs = truth
👉 Not guessing, not assumptions

🔥 FIX

Correct YAML:

env:
  - name: PORT
    value: "3000"

Apply again:

kubectl apply -f deployment.yaml
🎯 VERIFY
kubectl get pods

👉 Should be:

Running ✅
🐧 LINUX COMMANDS (VERY IMPORTANT)
🔍 1. grep → filter logs
kubectl logs <pod> | grep error

👉 Find only errors

📡 2. tail -f → live logs
kubectl logs -f <pod>

👉 Real-time logs (VERY IMPORTANT)

📖 3. less → read large logs
kubectl logs <pod> | less

👉 Scroll:

↑ ↓
q to quit
🧠 REAL DEVOPS SCENARIOS
🧠 Scenario 1: App crashing
kubectl logs <pod>

👉 Find error → fix env

🧠 Scenario 2: High traffic bug
kubectl logs -f <pod>

👉 Watch live logs

🧠 Scenario 3: Large logs
kubectl logs <pod> | grep "500"

👉 Find API errors

💣 BONUS DEBUG COMMANDS
Get all pods with status
kubectl get pods -o wide
Check events
kubectl get events
🎯 DAY 6 CHECKLIST
 Broke your app intentionally
 Saw CrashLoopBackOff
 Used describe
 Used logs
 Fixed issue
 Used grep / tail / less
🧠 MOST IMPORTANT LEARNING

👉 DevOps is NOT:

❌ writing YAML
❌ deploying apps

👉 DevOps IS:

✅ debugging failures
✅ reading logs
✅ fixing fast

🚀 NEXT STEP

Day 7 = 🔥 Mini Project

👉 You’ll deploy:

Frontend
Backend
Connect both
