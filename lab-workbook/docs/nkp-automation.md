# Automation: GitOps for software delivery at scale

**วัตถุประสงค์:** วัตถุประสงค์ของบทนี้คือการสำรวจความแตกต่างระหว่างกระบวนการ manual deployment และ deployment ที่อิงตาม GitOps สิ่งนี้จะช่วยให้คุณเห็นคุณค่าของ automation และประสิทธิภาพที่ GitOps นำมาสู่การจัดการ Kubernetes applications

!!! note
    หากคุณไม่คุ้นเคยกับ GitOps คุณสามารถดูได้ที่ [CNCF glossary](https://glossary.cncf.io/gitops/)

**สิ่งที่คุณจะได้ทำ:**

1.  **Set up GitOps Deployment**
    
    -   deploy applications ได้อย่างง่ายดายโดยใช้แค่ Git repository ที่เป็นแหล่งโฮสต์ Kubernetes manifests

2.  **Automate Multicluster Application Deployments**
    
    -   ลดความซับซ้อนในการจัดการ distributed environments ขนาดใหญ่ในระดับสเกลด้วยการ assign clusters ให้กับ GitOps deployments แบบไดนามิกโดยไม่ต้องมีการแทรกแซงแบบ manual
    
3.  **Recap and next steps**
    
    -   เมื่อคุณได้เรียนรู้วิธีการทำ application deployment แบบอัตโนมัติในสเกลระดับหลาย clusters แล้ว ก็สามารถขยับไปยังส่วนสรุป (conclusion) ได้เลย!
