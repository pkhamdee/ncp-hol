# NKP Advanced Hands-on Lab

Expected chapter duration

60 minutes

# [#](#fundamentals-basic-nkp-concepts) Fundamentals: Basic NKP concepts

**Objective:** The chapter equips you with foundational skills for working with NKP environments. By completing these tasks, you will gain practical insights into core NKP features.

**What You’ll Do:**

1.  **NKP quick tour**
    
    -   Even if you have seen the NKP console before, we recommend you take this quick tour to get familiar how is the layout, and to find faster the resources you are looking for
2.  **Deploy and expose a simple application**
    
    -   Launch your first basic app on Kubernetes and use kubectl!
    -   You’ll learn how to use Kubernetes services to expose your application for testing
3.  **Expose the sample application on production**
    
    -   A Service type _NodePort_ isn't reliable or scalable for production. Instead, let's use an `Ingress` and a `LoadBalancer`
4.  **Preparing for multi-tenancy**
    
    -   Dedicated Kubernetes clusters (hard multi-tenancy) can become expensive. Another option is to share clusters (soft multi-tenancy) among several teams
5.  **Persistent storage**
    
    -   Persistent storage is crucial in Kubernetes for managing stateful workloads allowing data to persist beyond pod restarts
6.  **Recap and next steps**
    
    -   Now that you’ve learned more about core NKP features, you’ll be ready to discover some key features NKP provides for managing day-2 operations for infrastructure and applications