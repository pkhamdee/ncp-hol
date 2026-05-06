# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 10 minutes

# [#](#expose-app-on-production) Expose app on production Lab

Right now our application is not exposed on a reliable and scalable way. Worker nodes are considered ephemeral, if the node is redeployed and the address changes, the application becomes inaccessible through that node.

In this lab you will:

-   Migrate your application service to `LoadBalancer` with **MetalLB**
-   Expose HTTP applications using `Ingress` with **Traefik**