# 🌐 Splunk Apache Log Visualizer  

A cybersecurity project focused on **Apache web server log analysis** using **Splunk dashboards**.  
This lab demonstrates how to monitor web requests, analyze response patterns, detect client/server errors, and visualize hits from different countries.  

---

## 🎯 Objective  
To build a **Splunk dashboard** that provides clear insights into Apache web traffic — helping identify trends, request sources, and possible anomalies.  

---

## 🧩 Lab Setup  
- **Tool:** Splunk Enterprise  
- **Dataset:** `apache_logs.json`  
- **Host:** `kali`  
- **Sourcetype:** `_json`

- ---

## ⚙️ Task 0: Setting up Time Range  

### 🕒 Add Time Range Input  
1. Go to **Edit Dashboard** → **Add Input** → Select **Time**  
2. Set **Label:** `Time Range`  
3. Set **Token:** `time_range`  
4. Add a **Submit** button  
5. Use `time_range` in every panel for consistent filtering  

---

## 📊 Task 1: Apache Traffic Overview  
**Goal:** Get a quick overview of how the web server is performing.  

### 1️⃣ Total Web Requests  
```spl
source="apache_logs.json" host="kali" sourcetype="_json"
| stats count as "Total Web Requests"
```

### 2️⃣ Successful Responses (HTTP 200)  
```spl
source="apache_logs.json" host="kali" sourcetype="_json" 
| where status>=200 AND status<300
| stats count AS "Successful Response"
```

### 3️⃣ Client Errors (HTTP 4xx)  
```spl
source="apache_logs.json" host="kali" sourcetype="_json"
| where status>=400 and status<500
| stats count AS "Client Errors"
```

### 4️⃣ Server Errors (HTTP 5xx)  
```spl
source="apache_logs.json" host="kali" sourcetype="_json"
| where status>=500 and status<600
| stats count AS "Server Errors"
```

---

## 📈 Task 2: Request & Source Analysis  
**Goal:** Understand which pages are most active and where requests are coming from.  

### 1️⃣ Top Requested URIs  
```spl
source="apache_logs.json" host="kali" sourcetype="_json"
| stats count by uri
```

### 2️⃣ Requests by IP Address  
```spl
source="apache_logs.json" host="kali" sourcetype="_json"
| stats count by ip
```

---

## 🌍 Task 3: Geo-Location Analysis  

### Requests by Country (Choropleth Map)  
```spl
source="apache_logs.json" host="kali" sourcetype="_json" method=GET
| table ip
| iplocation ip
| stats count by Country
| geom geo_countries featureIdField="Country"
```

---

## 🖼 Dashboard Screenshots  

<img width="1920" height="1020" alt="Screenshot 2025-11-01 131003" src="https://github.com/user-attachments/assets/335f0c65-f2e7-413e-9c3f-c0ae51628f07" />
<img width="1920" height="1020" alt="Screenshot 2025-11-01 131010" src="https://github.com/user-attachments/assets/020d78c3-4dee-4c96-b73e-77d111e6c594" />
<img width="1920" height="1020" alt="Screenshot 2025-11-01 131026" src="https://github.com/user-attachments/assets/424753c3-bd47-43ef-a44a-5c355400fe60" />

---

## 🏁 Conclusion  
This project helped me:  
- Visualize Apache web server activity using Splunk  
- Detect HTTP errors and request trends  
- Map client IPs to their geographical origins  

---

## 🔖 Tags  
`#Splunk` `#CyberSecurity` `#SIEM` `#ApacheLogs` `#SOC` `#WebSecurity` `#LearningByDoing`
