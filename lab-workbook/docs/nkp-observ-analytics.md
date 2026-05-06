# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 5 minutes

# [#](#nkp-insights) NKP Insights Lab

NKP Insights is a unified platform designed to detect and prevent cluster anomalies across hybrid cloud environments. It employs an Insights engine to gather critical events and metrics, which are then processed by the NKP Management Cluster.

An NKP Insight alert consists of:

-   Anomaly description
-   Root cause analysis
-   Solution
-   Best practices to avoid similar issues in the future

Did You Know?

**NKP Insights** is available exclusively with NKP Ultimate.

#### [#](#analyse-an-nkp-insight-alert) Analyse an NKP Insight Alert

1.  Start by opening the Insights dashboard for the _Default_ workspace and explore the various types of anomalies.
    
    ![NKP Insights](/cloudnative/assets/insights_1.6721dd38.png)
    
2.  Filter out by _Configuration_ issues and select a configuration anomaly related to _CPU throttling_, or if you find something interesting, choose one of the alerts to get more details.
    
    ![CPU throttling alert for insights](/cloudnative/assets/insights_4.95c8633c.png)
    
3.  Review the root cause analysis, solution, and best practices for this specific alert. If you selected a CPU throttling anomaly, you can also check the Prometheus monitoring dashboard and the Kubernetes-specific dashboard related to this alert for a deeper understanding.
    
    Note
    
    We are using Prometheus as an example on how NKP Insights integrates with the different tools included in NKP. There is not additional task for you once you open Prometheus or any other NKP dashboard.
    
    ![NKP Insights](/cloudnative/assets/Insights_7.0bcfac6e.gif)
    

In summary, NKP Insights is an essential tool for enhancing the security and efficiency of Kubernetes environments. It enables organizations to address issues before they escalate, ensuring compliance with best practices and improving operational performance. By leveraging Insights, platform teams can reduce downtime and achieve cost savings while focusing on delivering value to users.