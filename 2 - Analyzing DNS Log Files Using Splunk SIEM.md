# Analyzing DNS Log Files Using Splunk SIEM

## Introduction
<p>DNS (Domain Name System) logs provide vital insights into network activity and can help identify potential security threats. Using Splunk SIEM, you can effectively analyze DNS logs to detect anomalies, uncover malicious activities, and gain actionable intelligence for securing your network.</p>

## Prerequisites
<p>Before you begin analyzing DNS logs in Splunk, ensure the following:</p>

- Splunk is installed and configured.
- DNS log data sources are set up to forward logs to Splunk.

## Steps to Upload Sample DNS Logs to Splunk SIEM

1. **Prepare Sample DNS Log Files**
    - Obtain a sample [DNS log file](https://www.secrepo.com/maccdc2012/dns.log.gz) in a supported format (e.g., text file).
    - Verify that the log includes relevant fields such as source IP, destination IP, domain name, query type, and response code.
      <p align="center">
      <img src="https://i.imgur.com/SFXS0qm.png" height="80%" width="80%" alt="Splunk"/>
      </p>
    - Save the sample log file in a directory accessible by Splunk.

2. **Add Log Files to Splunk**
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

3. **Select File**
    - Click Select File and upload the prepared DNS log file.
      <p align="center">
      <img src="https://i.imgur.com/ih9XCzL.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
4. **Specify Source Type**
    - In the Set Source Type section, select the appropriate source type for DNS logs (e.g., ``dns`` or a custom type).
      <p align="center">
      <img src="https://i.imgur.com/JeAorcb.png" height="80%" width="80%" alt="Splunk"/>
      </p>

5. **Review Configuration**
    - Review settings such as index, host, and source type to ensure they match the uploaded log file.
      <p align="center">
      <img src="https://i.imgur.com/Lu3Bxxx.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      <p align="center">
      <img src="https://i.imgur.com/Pdo313t.png" height="80%" width="80%" alt="Splunk"/>
      </p>

6. **Upload Log File**
    - Click Review to confirm settings, then click Submit to upload the log file.
      <p align="center">
      <img src="https://i.imgur.com/57OMdUx.png" height="80%" width="80%" alt="Splunk"/>
      </p>

7. **Verify Upload**
    - Click on Start Searching to ensure DNS events are visible in Splunk.
      <p align="center">
      <img src="https://i.imgur.com/wxw9JLy.png" height="80%" width="80%" alt="Splunk"/>
      </p>

## Steps to Analyze DNS Log Files in Splunk SIEM

1. **Retrieve DNS Events**
    - In the Splunk search bar, use a query to find DNS events:
      ```
      source="dns.log.gz" host="LYCA-LAPTOP" sourcetype="dns"
      ```
      <p align="center">
      <img src="https://i.imgur.com/fe1JNr3.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
2. **Parsing DNS Data**
    - Click on Extract New Fields to create custom fields for analysis.
      
    - Select a sample log entry and choose Regular Expression or Delimiter for field extraction.
      <p align="center">
      <img src="https://i.imgur.com/MAMRkUN.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      <p align="center">
      <img src="https://i.imgur.com/P1OhbRp.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
    - Assign names to extracted fields (e.g., ``src_ip``, ``dst_ip``, ``domain``).
      <p align="center">
      <img src="https://i.imgur.com/tuKteeE.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
    - Click Finish, then verify new fields under Interesting Fields.
      <p align="center">
      <img src="https://i.imgur.com/snQgvwA.png" height="80%" width="80%" alt="Splunk"/>
      </p>

3. **Extract Key Fields**
    - Focus on fields like source IP, destination IP, domain name, query type, and response code.
    - Use regex to extract DNS-related data:
      ```
      index=* sourcetype=dns | regex _raw="(?i)\b(dns|port 53)\b"
      ```
      <p align="center">
      <img src="https://i.imgur.com/ejJiniQ.png" height="80%" width="80%" alt="Splunk"/>
      </p>
      
4. **Detect Anomalies**
    - Analyze unusual patterns or spikes in DNS activity with a query like:
      ```
      index=* sourcetype=dns | stats count by fqdn  
      ```
      <p align="center">
      <img src="https://i.imgur.com/sdMWPCB.png" height="80%" width="80%" alt="Splunk"/>
      </p>

      ### Potential Security and Operational Insights:

        1. **High Volume of Null or Corrupt Entries**
           - The presence of ``\x00\x00\x00...`` (10,401 occurrences) and (empty) (2,757 occurrences) could indicate:
             - Malformed DNS queries or corrupt data in the logs.
             - A misconfiguration in DNS logging or issues with Splunk's parsing of certain events.
             - Possible attempts at evasion or obfuscation from a malicious actor.
        
        2. **Suspicious or Unusual Domains**
           - ``s4yjzahnzaa.-connect.rssfeeds.com`` and auth.rssfeeds.com could be:
             - Legitimate RSS feed update checks.
             - Potential Command & Control (C2) domains used in malware.
           - ``.nessus`` suggests activity from a Nessus vulnerability scanner, which could mean:
             - A scheduled scan from a security team.
             - An unauthorized scan, if unexpected.
               
        3. **Reverse DNS Queries (PTR Records)**
           - Many entries like ``22.168.192.in-addr.arpa`` indicate reverse DNS lookups.
           - If an unusually high number of reverse lookups are happening, this could suggest:
             - Network reconnaissance or mapping activities.
             - Misconfigured services generating excessive lookups.

        4. **Facebook-Related Queries**
           - The domain ``o-jf-w.channel.facebook.com`` (1245 queries) suggests:
             - High traffic from users accessing Facebook services.
             - Could be normal behavior, but if unexpected, it warrants investigation.

      ### Recommendations:

        1. **Investigate the High Volume of Null/Corrupt Entries:**
           - Check if your Splunk parsing is correctly handling DNS logs.
           - Identify whether these entries are from specific IP sources or patterns.
           - If these are from external sources, consider blocking traffic from them.

        2. **Analyze Potentially Suspicious Domains:**
           - Check if ``rssfeeds.com`` activity aligns with expected internal behavior.
           - Cross-reference ``s4yjzahnzaa.-connect.rssfeeds.com`` in threat intelligence feeds.

        4. **Validate Nessus Activity:**
           - If Nessus scans are unexpected, verify if unauthorized security testing is occurring.
           - Cross-check the source IP addresses.

        5. **Monitor Reverse DNS Lookups:**
           - If excessive, investigate what is triggering them.
           - Determine if they correlate with internal services or external threats.

        6. **Check for Facebook Traffic Justification:**
           - Ensure this traffic volume is normal.
           - If seen on servers or restricted environments, investigate further.
      
5. **Identify Top DNS Sources**
   - Use the top command to identify the most frequent query types or sources:
      ```
      index=* sourcetype=dns | top fqdn, src_ip
      ```
      <p align="center">
      <img src="https://i.imgur.com/Ee317a4.png" height="80%" width="80%" alt="Splunk"/>
      </p>

      ### Potential Security and Operational Insights:

        1. **Teredo IPv6 Tunneling – Potential Concern**
           - ``teredo.ipv6.microsoft.com`` (27,425 queries, 6.48%)
           - Source IP: ``10.10.117.210``
           - Teredo is an IPv6 tunneling protocol used to encapsulate IPv6 packets in IPv4.
           - This could be legitimate Windows system behavior, but excessive queries might indicate:
             - Unnecessary tunneling, which could bypass security controls.
             - Potential command-and-control (C2) traffic if abused by malware.
             - Consider blocking or monitoring Teredo if not required in the environment.
        
        2. **Apple Services Activity**
           - ``www.apple.com`` (10,852 queries, 2.56%)
           - ``time.apple.com`` (6,038 queries, 1.43%)
           - Both are common Apple endpoints for system updates, authentication, and network time synchronization (NTP).
           - If these queries are unexpected on non-Apple devices, investigate further.
               
        3. **Google & Gmail Queries – Productivity or Suspicious?**
           - ``tools.google.com`` (10,179 queries, 2.40%)
           - ``imap.gmail.com`` (5,543 queries, 1.31%)
           - Frequent lookups indicate high user engagement with Google tools and email.
           - If appearing on restricted networks (e.g., servers, admin networks), investigate for unauthorized usage.
             
        4. **Reverse DNS Lookups (PTR Records)**
           - ``44.206.168.192.in-addr.arpa`` (7,248 queries, 1.71%)
           - Reverse DNS lookups (PTR queries) are often used for:
             - Network troubleshooting and logging
             - Security monitoring (e.g., SIEM tools resolving IPs)
           - High volume of PTR lookups from internal IPs may suggest:
             - Excessive system logging or poorly optimized configurations.
             - Malware attempting to map the network.
               
        5. **HPEBAA67 – Unusual Entry**
           - Appears as a hostname rather than a domain.
           - If it does not belong to your infrastructure, investigate:
             - Possible unauthorized hardware (e.g., rogue device).
             - Check logs for associated MAC addresses.
               
        6. **WPAD Queries – Possible Security Concern**
           - Web Proxy Auto-Discovery Protocol (WPAD)
           - 5,175 queries (1.22%) from 192.168.202.76
           - WPAD is used to auto-configure proxies for web traffic.
           - Security Risk: Attackers can exploit WPAD to redirect traffic through malicious proxies.
           - If WPAD is not required in your network, consider blocking WPAD DNS queries.
               
        7. **Social Media API Traffic – Acceptable or Policy Violation?**
           - ``api.twitter.com`` (4,163 queries, 0.98%)
           - ``api.facebook.com`` (4,137 queries, 0.98%)
           - High volume API queries suggest active social media usage.
           - If this activity is against corporate policy, further investigation is warranted.

      ### Recommendations:

        1. **Investigate Teredo Traffic**
           - If WPAD is not used, block WPAD resolution via DNS.
           - Ensure rogue proxy servers are not being introduced.

        2. **Review WPAD Traffic**
           - Check if ``rssfeeds.com`` activity aligns with expected internal behavior.
           - Cross-reference ``s4yjzahnzaa.-connect.rssfeeds.com`` in threat intelligence feeds.

        4. **Audit Reverse DNS Lookups**
           - Check which systems are performing excessive reverse lookups.
           - Determine if logging settings need optimization.

        5. **Validate HPEBAA67**
           - Confirm if this hostname belongs to an authorized device.
           - Review MAC address logs for anomaly detection.
             
        6. **Monitor High-Volume Social Media API Access**
           - If against policy, consider blocking access to social media APIs at the firewall.
           - If allowed, ensure traffic is not originating from unauthorized systems.
