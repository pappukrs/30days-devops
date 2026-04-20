🚀 DAY 10 — HPA (Horizontal Pod Autoscaler)
🎯 Goal
👉 Your app should:


Scale up when traffic/CPU ↑


Scale down when idle ↓



🧠 CORE CONCEPT (VERY IMPORTANT)
🔥 What is HPA?
👉 Horizontal Pod Autoscaler
More load → more podsLess load → fewer pods

🧠 HOW IT WORKS
Metrics (CPU/Memory)        ↓HPA Controller        ↓Adjust replicas in Deployment

🎯 Example
CPU > 70% → scale from 2 → 5 podsCPU < 20% → scale from 5 → 2 pods

⚠️ REQUIREMENT (IMPORTANT)
👉 HPA needs:


Metrics Server installed



✅ Check if installed
kubectl get deployment metrics-server -n kube-system
👉 If not found → install:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

🚀 STEP 1 — ADD RESOURCE LIMITS (MANDATORY)
HPA won’t work without this ❌

Update your deployment:
resources:  requests:    cpu: "100m"  limits:    cpu: "500m"

🔥 Full example
containers:- name: backend  image: pappukrs/devops-30-day2  resources:    requests:      cpu: "100m"    limits:      cpu: "500m"

Apply:
kubectl apply -f deployment.yaml

🚀 STEP 2 — CREATE HPA

Command (easy way)
kubectl autoscale deployment backend \  --cpu-percent=50 \  --min=1 \  --max=5

OR YAML way
apiVersion: autoscaling/v2kind: HorizontalPodAutoscalermetadata:  name: backend-hpaspec:  scaleTargetRef:    apiVersion: apps/v1    kind: Deployment    name: backend  minReplicas: 1  maxReplicas: 5  metrics:  - type: Resource    resource:      name: cpu      target:        type: Utilization        averageUtilization: 50

🚀 STEP 3 — VERIFY
kubectl get hpa
👉 Output:
NAME          CPU(%)   MIN   MAX   REPLICASbackend-hpa   20%      1     5     1

🔥 STEP 4 — GENERATE LOAD (IMPORTANT)
👉 Now we simulate traffic

Use curl loop
while true; do curl http://<external-ip>/api; done

OR run multiple requests
for i in {1..1000}; do curl http://<external-ip>/api & done

📈 STEP 5 — WATCH SCALING
kubectl get hpa -w
AND:
kubectl get pods

👉 You should see:
Pods: 1 → 2 → 3 → 4 🚀

🧠 WHAT’S HAPPENING
High CPU → HPA triggers → Deployment increases replicas

🐧 LINUX (USE TODAY)

🔹 Monitor CPU
top

🔹 Generate load (background)
yes > /dev/null &
👉 (CPU load simulation)

💣 DEBUG SCENARIOS

❌ HPA not scaling
Check:
kubectl top pods
👉 If error:


metrics-server not installed ❌



❌ CPU always 0
👉 Your app is too light
👉 Need more load

❌ No scaling
Check:


resource requests set?


correct deployment name?



🎯 DAY 10 CHECKLIST


 Metrics server installed


 Resource limits added


 HPA created


 Load generated


 Pods scaled automatically



🧠 INTERVIEW GOLD
Say this:

“HPA automatically scales pods based on CPU or memory metrics using the metrics server, adjusting replicas dynamically based on load.”

🔥 Strong answer

🚀 REAL INDUSTRY INSIGHT
👉 Companies combine:


HPA (pods scaling)


Cluster Autoscaler (nodes scaling)



🚀 NEXT STEP
If you want:
👉 I can show:


HPA + Ingress + Load test (real production simulation)


OR move to Helm (Day 8/9 advanced)



👉 NOW YOUR TURN
Reply:
👉 "HPA working"
OR paste your output → I’ll debug scaling with you 🔥