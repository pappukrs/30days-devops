Chal Pappu, ek bada visual bana ke samjhata hoon — Day 3 ka full concept ek interactive diagram mein. Step by step dekh.Ab thoda aur clearly break kar deta hoon key concepts:

---

**Problem recap (top section)**

Pods ka IP dynamic hota hai — restart hone pe naya IP milta hai. Toh koi bhi service directly pod IP pe depend nahi kar sakti. Yahi problem Service solve karti hai.

---

**Service ka magic: label selector**

Service pod ko IP se nahi, **label** se dhundhti hai. Tera YAML mein `selector: app: video-backend` hai, aur Deployment mein bhi `labels: app: video-backend` hoga — jab dono match karte hain, traffic automatically us pod ko milta hai. Pod crash ho, naya bane, IP badlo — Service ko farak nahi padta.

---

**Teen types — ek line mein**

- `ClusterIP` — sirf cluster ke andar. Backend-to-DB, frontend-to-backend wali calls ke liye.
- `NodePort` — node ka ek port open ho jaata hai. Testing ke liye thik hai, production mein avoid karo.
- `LoadBalancer` — GCP ek real external IP assign karta hai. Yahi production mein use hota hai.

---

**YAML ke 3 important fields**

| Field | Kya karta hai |
|---|---|
| `selector` | Kaunse pods ko traffic bhejni hai (label match) |
| `port: 80` | User/internet jo port hit karta hai |
| `targetPort: 3000` | Container ke andar Node.js kis port pe sun raha hai |

Ye confusion ka sabse bada point hai — `port` bahar ka hai, `targetPort` andar ka. Tera app `3000` pe chalta hai, but user `80` pe aata hai.

---

**Commands yaad rakh**

```bash
kubectl apply -f service.yaml   # service create karo
kubectl get svc                 # External-IP aa gaya kya? (1-2 min wait karo)
kubectl describe svc video-backend-service   # debug ke liye
```

