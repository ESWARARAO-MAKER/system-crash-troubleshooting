## System Crash Troubleshooting
In this we are going to troubleshoot the System Crash

## Overview
When a system crashes, it's important to have a well-structured approach to identify, rectify, and track the issue. Here’s a step-by-step guide on how to handle such a situation:

- **1. Initial Response:**
  - **Stay Calm:** Panicking can lead to mistakes; staying calm helps you think clearly.
    - **Eg:** If an e-commerce site like Amazon faces a sudden crash during a sale, staying calm allows the team to address the issue efficiently instead of creating panic and confusion.
  - **Document the Incident:** Note down the time of the crash and any actions taken to help with later analysis.
    - **Eg:** If your website goes down at 3 PM, note this time and log all activities as you attempt to troubleshoot, which will help during the post-mortem analysis.
  - **Communicate with Stakeholders:** Inform relevant teams or clients about the issue to manage expectations.
    - **Eg:** Notify clients or teams via a pre-set communication channel like Slack or email to let them know you are aware of the problem and working on it.
 
- **2. Analyze and Identify the Fault:**
  - **Check Logs:** Review system logs for any errors or warnings leading up to the crash. Use tools like ELK Stack (Elasticsearch, Logstash, Kibana), Splunk, or local logs (e.g., syslog, application logs).
    - **Example:** For a Node.js application, check the application.log file or use a tool like PM2 to view logs:
      ```
      pm2 logs
      ```
    - Use ELK Stack to query and visualize logs that may show an increase in error rates before the crash.
      
  - **Monitor System Metrics:** Review CPU, memory, and disk usage using monitoring tools such as Prometheus, Grafana, or Datadog to check if resource exhaustion played a role.
    - **Example:** If using Datadog, set up dashboards that show CPU usage spikes. If the crash correlates with high CPU usage at 2:30 PM, you’ve likely identified an issue with resource exhaustion.
      
  - **Review Recent Changes:** Identify any code changes, updates, or deployments that might have impacted the system.
    - **Example:** Check GitHub or GitLab for the latest commits to see if recent deployments caused the crash.
      
  - **Crash Reports:** Use built-in crash reporting tools or services like Sentry or Bugsnag for applications to get detailed stack traces.
    - **Example:** For a React web app, integrate Sentry to automatically capture detailed crash reports with stack traces.
    ```
    Sentry.init({ dsn: 'your-dsn-url' });
    ```
 
- **3. Contain the Issue:**
  - **Roll Back Changes:** If the crash was due to a recent deployment or change, consider rolling back to the last stable version.
    - **Example:** Use git revert to undo recent changes and redeploy the last stable version:
    ```
    git revert HEAD
    ```
  - **Isolate the Problem:** If possible, isolate the affected parts of the system to prevent further damage or data loss.
    - **Example:** If a microservice architecture is used, isolate the affected service (e.g., payment-service) using Kubernetes to prevent cascading failures.
    ```
    kubectl cordon payment-service-node
    ```
 
- **4. Rectify the Fault:**
  - **Debug and Fix Code Issues:** If a software bug is identified, debug and patch the code. Use techniques like step-by-step debugging and unit testing to verify fixes.
    - **Example:** If a crash is due to a NullPointerException in Java
    ```
    if (object != null) {
        object.method();
    }
    ```
    - Patch the code and test with unit tests.
      
  - **Address Hardware Issues:** If the problem is hardware-related, involve the IT team to replace or repair faulty components.
    - **Example:** Replace a failed SSD on a server if monitoring tools show disk errors.
      
  - **Update Configurations:** Ensure proper system configurations and environment variables to prevent similar issues in the future.
    - **Example:** Ensure correct database connection limits are set in a Node.js app configuration file:
    ```
    pool: {
      max: 10, // Avoids overloading the database
    }
    ```
  - **Patch Security Vulnerabilities:** If the crash was due to an attack or security flaw, patch the vulnerability and apply necessary updates.
    - **Example:** If an attacker used an SQL injection to crash your database, update your code to use parameterized queries:
    ```
    db.query('SELECT * FROM users WHERE id = ?', [userId]);
    ```
 
- **5. Implement Fault Tracking and Prevention:**
  - **Set Up Monitoring and Alerts:** Implement real-time monitoring solutions that provide alerts when system resources reach critical levels.
    - **Example:** Configure Prometheus and Grafana for alerting when memory usage exceeds 80%:
    ```
    alert: HighMemoryUsage
    expr: node_memory_Active_bytes / node_memory_MemTotal_bytes > 0.8
    ```
  - **Create Incident Reports:** Document the issue thoroughly, including root cause, steps taken, and resolution, for future reference.
    - **Example:** Document the issue in a report template:
    ```
    Incident ID: 00123
    Summary: System crash due to out-of-memory error.
    Root Cause: Memory leak in the user-authentication service.
    Resolution Steps: Fixed memory leak, redeployed service.
    ```
  - **Run Post-Mortem Analysis:** Review the incident with your team to understand what went wrong and how similar incidents can be prevented.
    - **Example:** Hold a meeting to discuss a recent server crash. Identify causes and preventative measures, such as code reviews or load testing.
  - **Plan for Redundancy:** Use high availability solutions like load balancing, failover systems, or distributed cloud architectures to minimize downtime.
    - **Example:** Use AWS Load Balancer to ensure traffic switches to a healthy instance if one server fails.
 
- **6. Test and Validate:**
  - **Stress Test:** Run stress tests on the system to ensure it can handle load and recover gracefully from potential crashes.
    - **Example:** Run Apache JMeter to simulate 1,000 concurrent users on a web app to ensure it handles high traffic.
  - **Regression Testing:** Ensure the fix did not introduce new issues.
    - **Example:** Run automated tests after applying patches to confirm no new issues are introduced.
    ```
    npm run test
    ```
    
  - **Automate Fault Detection:** Implement automated scripts or tools that routinely check for system anomalies.
    - **Example:** Set up a Shell script to check for server status every minute and alert if it goes down.
    ```
    while true; do
        if ! curl -s http://your-app.com; then
            echo "App is down" | mail -s "Alert" admin@example.com
        fi
        sleep 60
    done
    ```

## Tools and Technologies to Aid in Fault Rectification:**
  - **Log Management:** ELK Stack, Splunk, Graylog
  - **Monitoring and Metrics:** Prometheus, Grafana, Datadog, New Relic
  - **Crash Reporting:** Sentry, Bugsnag, Rollbar
  - **Performance Testing:** Apache JMeter, Gatling, Locust
 
## Long-Term Strategies:
  - **Regular Backups:** Ensure consistent backups are in place to recover from data loss.
    - **Example:** Schedule daily backups with cron jobs
    ```
    0 2 * * * /usr/bin/pg_dump your_database > /backup/db_backup_$(date +\%F).sql
    ```
    
  - **Disaster Recovery Plan:** Develop a comprehensive recovery plan to quickly bring systems back online.
    - **Example:** Prepare a plan with clear recovery steps. Include using cloud failover capabilities like AWS Route 53 to reroute traffic.

  - **Training and Drills:** Conduct team training and simulate failure scenarios to improve readiness.
    - **Example:** Organize quarterly failure simulation drills to keep the team prepared.
