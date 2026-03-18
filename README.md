# CS628_final_project_SIEM_model
This project is a Mini SIEM (Security Information and Event Management) system built using the ELK Stack (Elasticsearch, Logstash, Kibana) to monitor, analyze, and visualize SSH login activity.

It ingests system authentication logs, detects suspicious behavior (like brute-force attacks), and presents insights through a live Kibana dashboard with geospatial visualization.

The files present here are just some connection files where all the logs are written in logs folder 

The generate_live_logs.py does the same it just builds up the logs linux ssh logs randomly into the files

The generate_alerts.py is used to provide real-time alerts if they exist in logs

While the logs data is parsed and sent to the kibana dashboard where the parsed data can be used to
present different type of analyzing tool and present the data

# To RUN the SIEM model
🏗️ Architecture
Auth Logs → Filebeat → Logstash → Elasticsearch → Kibana Dashboard

Filebeat → Reads system logs (auth.log)

Logstash → Parses logs, extracts fields, enriches with GeoIP

Elasticsearch → Stores structured logs

Kibana → Visualizes data (charts + maps)

⚙️ Setup Instructions
cd elk
2️⃣ Start ELK Stack
docker compose up -d

Check containers:

docker ps
3️⃣ Access Services

Elasticsearch → http://localhost:9200

Kibana → http://localhost:5601

4️⃣ Generate Logs (Testing)

Add logs to simulate SSH activity:(manually)

echo '2026 Jan 12 10:21:01 server sshd[1234]: Failed password for root from 185.220.101.50 port 22 ssh2' >> logs/auth.log

Add logs to simulate SSH activity:(generate_live_logs.py)

5️⃣ Create Data View in Kibana

Go to:

Kibana → Stack Management → Data Views

Create:

Index pattern: auth-logs-*
Time field: @timestamp

📌 Core Functionalities
1️⃣ Time Selection Panel

Interactive time filter in Kibana

Allows analysis of login attempts within a custom time window

2️⃣ Login Attempts Timeline

Line chart showing login attempts over time

Detect spikes indicating brute-force attacks

3️⃣ Top Attacking IPs (Top-K Analysis)

Bar chart of IP addresses with most login attempts

Helps identify malicious sources

4️⃣ Geolocation-Based Attack Map 🌍

Maps IP addresses using GeoIP enrichment

Displays attack origin locations worldwide

Supports clustering for attack hotspots

5️⃣ Historical Attack Correlation

Detects if current IPs:

appeared in past logs

belong to same subnet

Used for identifying repeat attackers or coordinated attacks

6️⃣ AI Insight Feed (Behavioral Analysis)

Custom logic identifies anomalies such as:

IP normally logs in once/hour → now multiple attempts

Sudden spike in failed logins

Abnormal activity pattern

Example insight:

⚠️ IP 185.220.101.50 shows unusual activity (6 failed attempts in 1 minute)
7️⃣ Multi-IP Username Detection

Detects same username logging from multiple IPs

Indicates possible:

credential compromise

distributed brute-force attack

8️⃣ Success vs Failure Analysis

Pie chart showing:

Successful logins

Failed logins
