# Scenario 2 - production set-up: 
Discuss and justify below points: 

1.	Recommendation as for the application environments (how many and why?) 

For the production setup, I recommend a minimum of two environments: the Production environment and the Staging environment. The Production environment is for the deployment of the final version of the application for end users, while the Staging environment is for testing purposes before deployment to Production; it will mirror the Production environment. However, from application development to deployment in Production, we need at least three environments, including the Development environment. In total, having at least three environments is ideal for a small application like a voting app to ensure that new features are developed, tested, and validated before being deployed to end users.
1.1	how are they going to be separated?

By the branching strategy and automated CI/CD pipeline that can deploy the application to the corresponding environment. 
                      Branching Strategy
To efficiently manage the app deployment, we need to ensure proper code promotion through various environments, the following branching strategy is recommended:
Develop Branch
•	Purpose: used for developing and testing new features. It’s a collaborative space where developers integrate and test their code before it’s merged into the main branch.
The Code merged to this branch from feature branches initiates deployment to the DEV environment.
Staging Branch
•	Purpose: This branch closely mirrors the production environment. It’s used for final testing before deployment to ensure everything works as expected in a production-like setting.
Main Branch
•	Purpose: Used for production-ready code. Code merged to this branch from Staging branch that initiates deployment to the PROD environment.







The following diagram represent the branching strategies. 
 
The following diagram illustrate the environments and the application deployment Model:
 
Note: I have chosen a Kubernetes cluster to deploy the application. If we do not choose to use a cluster, we can have separate instances for each environment. However, within the cluster, I have isolated the two environments by namespace because it is more cost-effective than maintaining a separate cluster for each environment. 

2.	 Which tooling will you use for CI/CD and why? 

I have selected GitHub Actions as the CI/CD tooling for this app deployment. GitHub Actions is chosen because it provides secure secret management for pipelines and integrates seamlessly with GitHub repositories, utilizing branch protections and taking advantage of the various pre-built actions available. Additionally, GitHub Actions' security can be enhanced by following GitHub's best practices for CI/CD.
2.1 How to ensure no downtime during deployments? 
To ensure no downtime during deployments, I will implement one of the following strategies:
Rolling Updates: This deployment strategy updates application instances gradually. By replacing instances one at a time, it minimizes disruption and allows for quick rollback if issues arise.
Blue/Green Deployment: this strategy involves running two environment, blue (current) and green (new). Traffic is gradually shifted from the blue to the green environment. However, it can incur additional costs for maintaining two environments simultaneously.
Canary Releases: For more controlled feature rollouts, this approach involves deploying changes to a small subset of users or instances first before a full-scale release. This helps identify and fix issues early while minimizing the impact on all users.
Recommendation: 
I recommend using canary releases specifically for the production setup because they are ideal for testing new features or significant changes with limited exposure before a full rollout. Furthermore, canary releases allow for zero downtime during the transition compared to Blue/Green deployment and Rolling Update. This approach is also cost-effective as it avoids maintaining multiple environments simultaneously.
3	What is the additional tooling you need to supplement the application with to ensure it runs smoothly on production? (e.g. from observability) 

1.	Prometheus, Loki, and Grafana monitoring stack: For the most robust monitoring, we can use the Prometheus, Loki, and Grafana monitoring stack.  Loki can setup for collecting the application logs and Prometheus for set up monitoring metrics and Grafana for monitoring application performance and alerting if the application is down via Alertmanager.

To react to the alerting notifications and roll back to the previous version, we can use:
 Automated Rollback
•	GitHub Actions: Create a workflow that triggers a rollback when an alert is received. This can be done by deploying the previous stable version of the application.
•	Kubernetes: Use Kubernetes’ built-in rollback capabilities to revert to a previous deployment.
2.	Application deployed in an AWS Cloud: CloudWatch Logs can be configured to collect application logs, and CloudWatch metrics can be set up to monitor the application's health. If the application goes down, a CloudWatch alarm will trigger and send a notification via AWS SNS Additionally, an AWS Lambda function can be invoked to automatically roll back to the previous version if needed, ensuring minimal disruption and quicker recovery.

4. Networking & DNS-records management and networking protection rules 

Networking:
•	For the networking setup of application, we can use AWS Network ACLs: Control traffic by allowing or denying specific inbound or outbound traffic at the subnet level.  
•	Load Balancers: Manage application loads in production.

DNS Records Management:
AWS Route 53: Manage DNS records, track DNS queries, and block malicious DNS queries.
Networking Protection Rules:
•	AWS Web Application Firewall (WAF): Protect the application from common web exploits (Layer 7). 

5. Code 
