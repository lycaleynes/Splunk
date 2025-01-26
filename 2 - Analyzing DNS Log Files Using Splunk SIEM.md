# Analyzing DNS Log Files Using Splunk SIEM

## Introduction
<p>DNS (Domain Name System) logs provide vital insights into network activity and can help identify potential security threats. Using Splunk SIEM, you can effectively analyze DNS logs to detect anomalies, uncover malicious activities, and gain actionable intelligence for securing your network.</p>

## Prerequisites
<p>Before you begin analyzing DNS logs in Splunk, ensure the following:</p>

- Splunk is installed and configured.
- DNS log data sources are set up to forward logs to Splunk.

## Steps to Upload Sample DNS Logs to Splunk SIEM

1. Prepare Sample DNS Log Files
    - Obtain a sample [DNS log file](https://www.secrepo.com/maccdc2012/dns.log.gz) in a supported format (e.g., text file).
    - Verify that the log includes relevant fields such as source IP, destination IP, domain name, query type, and response code.
      <p align="center">
      <img src="https://i.imgur.com/SFXS0qm.png" height="80%" width="80%" alt="Splunk"/>
      </p>
    - Save the sample log file in a directory accessible by Splunk.

2. Add Log Files to Splunk
    - Log in to the Splunk web interface.
      <p align="center">
      <img src="https://i.imgur.com/KgohSCB.png" height="80%" width="80%" alt="Splunk"/>
      </p>
    - Go to Settings > Add Data.
      <p align="center">
      <img src="https://i.imgur.com/B7fO885.png" height="80%" width="80%" alt="Splunk"/>
      </p>
    - Choose Upload as the data input method.
      <p align="center">
      <img src="https://i.imgur.com/cmPpxO6.png" height="80%" width="80%" alt="Splunk"/>
      </p>

3. Select File
    - Click Select File and upload the prepared DNS log file.
      <p align="center">
      <img src="https://i.imgur.com/JhrMgdl.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
4. Specify Source Type
    - In the Set Source Type section, select the appropriate source type for DNS logs (e.g., ``dns`` or a custom type).
      <p align="center">
      <img src="https://i.imgur.com/Ry8SQ5l.png" height="80%" width="80%" alt="Splunk"/>
      </p>

5. Review Configuration
    - Review settings such as index, host, and source type to ensure they match the uploaded log file.
      <p align="center">
      <img src="https://i.imgur.com/Lu3Bxxx.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      <p align="center">
      <img src="https://i.imgur.com/Pdo313t.png" height="80%" width="80%" alt="Splunk"/>
      </p>

6. Upload Log File
    - Click Review to confirm settings, then click Submit to upload the log file.
      <p align="center">
      <img src="https://i.imgur.com/57OMdUx.png" height="80%" width="80%" alt="Splunk"/>
      </p>

7. Verify Upload
    - Use the search bar to ensure DNS events are visible in Splunk:
      ```
      source="<File Name>" host="<Host>" sourcetype="<Source Type>"
      ```
      <p align="center">
      <img src="https://i.imgur.com/833b3i0.png" height="80%" width="80%" alt="Splunk"/>
      </p>

## Steps to Analyze DNS Log Files in Splunk SIEM

1. Retrieve DNS Events
    - In the Splunk search bar, use a query to find DNS events:
      ```
      index=* sourcetype=dns
      ```
      <p align="center">
      <img src="https://i.imgur.com/xp9hs6E.png" height="80%" width="80%" alt="Splunk"/>
      </p>

2. Extract Key Fields
    - Focus on fields like source IP, destination IP, domain name, query type, and response code.
    - Use regex to extract DNS-related data:
      ```
      index=* sourcetype=dns | regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"
      ```
      <p align="center">
      <img src="https://i.imgur.com/iMVuKxg.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
3. Detect Anomalies
    - Analyze unusual patterns or spikes in DNS activity with a query like:
      ```
      index=* sourcetype=dns | stats count by fqdn  
      ```
      <p align="center">
      <img src="https://i.imgur.com/iMVuKxg.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
5. Identify Top DNS Sources
Use the top command to identify the most frequent query types or sources:
css
Copy
Edit
index=* sourcetype=dns_sample | top fqdn, src_ip  
6. Investigate Suspicious Domains
Search for domains linked to malicious activity using threat intelligence feeds:
makefile
Copy
Edit
index=* sourcetype=dns_sample fqdn="maliciousdomain.com"  
