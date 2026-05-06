# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 5 minutes

# [#](#metrics-visualization) Metrics visualization Lab

NKP offers centralized observability for Kubernetes environments with 27 built-in Grafana dashboards and support for custom dashboards, enabling comprehensive monitoring and management across multi-cloud and hybrid setups.

Did You Know?

**Grafana** is included only with NKP Pro and Ultimate licensing.

In this exercise, you will explore Grafana dashboards to examine the WordPress application you previously deployed.

1.  Navigate to your specific project where the WordPress application is deployed.
    
    ![Navigating to Project for Graphana Dashboard](/cloudnative/assets/open_project.5889bf31.png)
    
2.  Open the Grafana dashboard for the cluster assigned to your project to check monitoring metrics.
    
    ![Click on Grafana](/cloudnative/assets/metrics_2.627c6a95.png)
    
3.  The default dashboard on the homepage displays resource utilization for your NKP cluster. From there, navigate to the built-in workload dashboard to check compute resources.
    
    ![Built-in workload dashboard](/cloudnative/assets/metrics_4.0c7f2b2e.png)
    
4.  Analyze application-specific metrics by using filters within the namespace field with your project. This will allow you to observe the performance of the WordPress application and its MySQL database by selecting the respective workload.
    
    ![WordPress application dashboard](/cloudnative/assets/metrics_3.5e5e8090.gif)
    

In summary, Grafana in NKP provides flexibility through custom and community-driven dashboards, enhanced troubleshooting with multi-source data integration, and centralized access control for secure metrics governance. This comprehensive approach enables efficient monitoring and management of Kubernetes environments.

Pro tip

There are built-in Grafana dashboards to monitor K8s components in addition to user deployed workloads. The following dashboard is an example for checking the availability of the Kubernetes API server. You can also easily change the time interval.

![K8s API dashboard](/cloudnative/assets/kube-api.d8058ec0.png)