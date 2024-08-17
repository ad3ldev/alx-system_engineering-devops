# Postmortem: The Day the E-Commerce Site Stood Still

## Issue Summary

- **Duration:** August 15, 2024, from 10:30 AM to 12:45 PM (UTC)
- **Impact:** The e-commerce web application experienced a complete outage, affecting 100% of users. Customers were unable to browse products, add items to their carts, or complete purchases. The outage resulted in an estimated loss of $50,000 in sales.
- **Root Cause:** A misconfiguration in the load balancer's SSL certificate caused it to reject incoming HTTPS requests, leading to a complete shutdown of web services.

---

## Timeline

- **10:30 AM:** Monitoring alerts triggered by a sudden drop in traffic and a spike in 5xx errors.
- **10:32 AM:** On-call engineer received a pager alert and began investigating.
- **10:35 AM:** Initial assumption was that the issue was related to a database slowdown due to the recent update.
- **10:40 AM:** Database team was notified; they confirmed that the database was functioning normally.
- **10:50 AM:** Investigation shifted to the application servers, suspecting a possible memory leak.
- **11:10 AM:** Application servers were rebooted, but the issue persisted.
- **11:25 AM:** Networking team was involved, focusing on potential load balancer issues.
- **11:35 AM:** Load balancer logs revealed SSL handshake failures.
- **11:40 AM:** Identified that the SSL certificate had been recently renewed, but the configuration had not been updated properly on the load balancer.
- **11:50 AM:** Corrected the SSL configuration and restarted the load balancer.
- **12:00 PM:** Services began to recover gradually.
- **12:45 PM:** Full service restoration confirmed; post-incident monitoring initiated.

---

## Root Cause and Resolution

**Root Cause:** The root cause of the outage was a misconfiguration in the load balancer's SSL certificate settings. The SSL certificate had been renewed, but the new certificate was not properly applied in the load balancer's configuration. As a result, the load balancer rejected all incoming HTTPS requests, leading to a complete outage of the web application.

**Resolution:** The issue was resolved by correctly applying the new SSL certificate to the load balancer's configuration and restarting the load balancer. Once the correct SSL certificate was in place, the load balancer was able to process HTTPS requests normally, and the web application services were restored.

---

## Corrective and Preventative Measures

**Improvements/Fixes:**
1. **Automate SSL Certificate Renewal:** Implement an automated process for SSL certificate renewal and deployment to ensure that the new certificates are correctly applied across all relevant services.
2. **Enhanced Monitoring:** Add specific monitoring checks for SSL certificate validity and load balancer configuration to catch similar issues before they cause an outage.
3. **Configuration Management:** Implement a more rigorous configuration management process to verify that all changes, especially critical ones like SSL updates, are applied correctly.

**Tasks:**
- **Automate SSL Deployment:** Develop a script to automate the deployment of SSL certificates to the load balancer.
- **Add Monitoring:** Implement monitoring alerts specifically for SSL handshake failures and certificate expiration.
- **Configuration Review Process:** Establish a mandatory peer review process for all configuration changes involving critical infrastructure components like load balancers.
- **Runbooks Update:** Update the runbooks with detailed steps for SSL renewal and load balancer configuration to ensure that future incidents can be resolved more quickly.

---

This postmortem outlines the key details and actions taken during the outage to help prevent future incidents and improve the reliability of the system.

