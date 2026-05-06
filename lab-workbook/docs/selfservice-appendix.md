# Glossary

มีแนวคิดในอุตสาหกรรมที่คุ้นเคยและ terms ที่ใช้ซ้ำกันมากมายซึ่งมีความหมายเฉพาะเมื่อใช้งานโดย Calm. อ้างอิง glossary ของ Calm terms นี้เพื่อช่วยในการเริ่มต้นค้นพบและทำความเข้าใจของคุณ. โปรดดูที่ [Additional Resources](#additional-resources) สำหรับการศึกษาเชิงลึกเพิ่มเติม!

## Calm Terms

#### Action

ชุดของ [task](#task) ที่ทำงานตามลำดับใน [blueprint](#blueprint). Basic actions ที่รวมอยู่ในทุก blueprint ประกอบด้วย: Create, Delete, Stop, Start, Restart. developer role สามารถสร้าง custom actions เพิ่มเติมได้ (เช่น runbook), scale-in และ scale-out actions, และ pre- และ post- create actions.

#### Application

deployment instance ของ [blueprint](#blueprint), และยังเป็นไอคอนเมนูระดับบนสุดของ Calm.

#### Application Profile

developer role สามารถรวมและตั้งชื่อชุดของ [blueprint](#blueprint) variables และ service creation definitions เป็น Application Profile. operator role สามารถเลือก application profile ระหว่างการ launch blueprint เพื่อเป็นตัวเลือกในการ deployment. ตัวอย่างเช่น: service resource sizes (ขนาดเล็ก, กลาง, หรือใหญ่), deployment scenarios (development, staging, หรือ production).

#### Blueprint

application model ของ [service](#service) topology ที่มีการจัดการ orchestrational [dependency](#dependency) ของ [action](#action) และ [task](#task), โดย blueprint จะต้องมีอย่างน้อยหนึ่ง [service](#service) และ [application profile](#application-profile) เริ่มต้น. และยังเป็นไอคอนเมนูระดับบนสุดของ Calm.

#### Brownfield

blueprint ที่ประกอบด้วย [existing machine](#existing-machine) ที่ถูกนำเข้ามาจาก [provider](#provider).

#### Calm

Calm คือ application lifecycle manager ซึ่งจัดเตรียม automation platform สำหรับการจำลอง business governance, applications, และ infrastructure ของคุณเข้าด้วยกันเป็น artifact เดียว: blueprint. ในปัจจุบัน Calm ถูกส่งมอบใน Nutanix [Prism Central](#prism-central) และสามารถเปิดใช้งานได้ด้วย one-click.

#### Dependency

การเชื่อมต่อตามลำดับเชิงตรรกะระหว่าง services ซึ่งจัดการ orchestrating ส่วน basic actions ของพวกมันเข้าด้วยกัน โดยแสดงด้วยลูกศรสีขาว. การเชื่อมต่อตามลำดับที่ถูกสร้างขึ้นหรือกำหนดโดย developer ระหว่าง [task](#task) (ข้าม services ใน action เดียวกัน), โดยแสดงด้วยส่วนโค้งสีส้ม.

#### Epsilon

workflow engine ของ Calm ซึ่งเป็น internal component ที่ถูกควบคุมโดย Life Cycle Manager และถูกใช้โดย [Prism Central](#prism-central) facilities อื่นๆ.

#### eScript (Epsilon Script)

ประเภทของ Calm [task](#task) สำหรับรัน local, restricted Python environment.

#### Existing Machine

existing service (ที่ไม่ได้ provisioned โดย Calm) ซึ่งสามารถถูกจัดการได้โดย [task](#task).

#### Kubernetes

ระบบ container management และ hosting, ดู [https://kubernetes.ioopen in new window](https://kubernetes.io)

#### Macro

ไม่ว่าจะเป็น [variable](#variable) แบบ built-in ของ Calm หรือที่ระบุใน blueprint ซึ่งสามารถใช้ใน [task](#task) และ variables ได้.

#### Marketplace

end-user, self-service portal ของ Calm blueprints ที่ถูก publish แล้ว ซึ่งควบคุมโดย projects และ roles ภายใน Prism Central. และยังเป็นไอคอนเมนูระดับบนสุดของ Calm.

#### Prism Central

Nutanix scale-out control plane สำหรับจัดการ Nutanix clusters ที่เชื่อมต่อกันหลายตัว และจัดเตรียม advanced management capabilities ([Calm](#calm), [Flow](#flow), [Karbon](#karbon), ฯลฯ) จาก single pane of glass web console.

#### Project

กลุ่มของ users หรือ groups ที่รวมเข้าด้วยกันซึ่งสัมพันธ์กับ [roles](#role) ที่สามารถเข้าถึง [providers](#Provider) ด้วย resource quotas. และยังเป็นไอคอนเมนูระดับบนสุดของ Calm.

#### Provider

infrastructure host ที่ให้บริการ [services](#service) พร้อมการระบุ show back resource costs. ระบุไว้ใน Calm settings.

#### Role

ชุดของ permissions ที่กำหนด abilities ของ user. ตัวอย่างเช่น: start a VM, create a Calm blueprint.

#### Runtime

runtime property อนุญาตให้ property ที่ระบุสามารถถูก override ได้เมื่อทำการ launch [blueprint](#blueprint) หรือ action.

#### Secret

credential password หรือ key, ซึ่งเป็น [application profile](#application-profile) หรือ service variable ที่ถูกระบุด้วย secret property. Secrets จะถูกระบุโดย developer [role](#role), พวกมันจะถูกซ่อนไว้เมื่อบันทึก blueprint เพื่อป้องกันการถูกขโมย. Secrets จะถูกทำให้ว่างเปล่า (blanked) เมื่อ blueprint ถูก export ออกไป และ secrets จะถูกทำให้ว่างเปล่าใน output ของ [task](#task).

#### Service

instance ของ [substrate](#substrate), โดยทั่วไปจะเป็น virtual machine, existing machine, หรือ [Kubernetes](#kubernetes) pod.

#### Substrate

infrastructure [provider](#provider) instance, ซึ่ง [blueprint](#blueprint) สามารถใช้ providers ทั้งหมดที่ถูก enable ใน [project](#project) ได้.

#### Task

ขั้นตอนการทำงาน (stage of operational execution) ส่วนบุคคลภายใน [action](#action).

#### Variable

[blueprint](#blueprint) property: ถูกตั้งค่าแบบ static โดย developer [role](#role) ด้วยค่า default value, ใช้เป็น macro ใน [task](#task), และถูกระบุด้วย [runtime](#runtime) property เพื่อมอบหมายการตั้งค่าให้กับ operator role ในระหว่างการ launch blueprint.

## Nutanix Products

#### Calm

Self-service application-centric automation และ management, ดู [Calm](#calm) และ [https://www.nutanix.com/products/calmopen in new window](https://www.nutanix.com/products/calm). ในปัจจุบัน Calm ถูกส่งมอบใน Nutanix [Prism Central](#prism-central) และสามารถเปิดใช้งานได้ด้วย one-click.

#### Era

Database-as-a-Service (DBaaS) automation และ data management, ดู [https://www.nutanix.com/products/eraopen in new window](https://www.nutanix.com/products/era).

#### Flow

Microsegmentation (distributed firewall) security และ management สำหรับ Nutanix cluster services, ดู [https://www.nutanix.com/products/flowopen in new window](https://www.nutanix.com/products/flow). Flow ถูกส่งมอบใน Nutanix [Prism Central](#prism-central) และสามารถเปิดใช้งานได้ด้วย one-click.

#### Karbon

Platform-as-a-service (PaaS), managed container offering สำหรับ [Kubernetes](#kubernetes), ดู [https://www.nutanix.com/products/karbonopen in new window](https://www.nutanix.com/products/karbon). ในปัจจุบัน Karbon ถูกส่งมอบใน Nutanix [Prism Central](#prism-central) และสามารถเปิดใช้งานได้ด้วย one-click.

#### Prism Central

ดู [Prism Central](#prism-central) และ [https://www.nutanix.com/products/prismopen in new window](https://www.nutanix.com/products/prism).

## Additional Resources

1.  \[Blog\] [Calm Terminologyopen in new window](https://next.nutanix.com/blog-40/calm-terminology-actions-and-dependencies-33852)[\[Source\]open in new window](https://github.com/MichaelHaigh/calm-blueprints/blob/master/DependencyTaskExample/README.md)
2.  [Nutanix Calm Admin Operations Guide: Major Componentsopen in new window](https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v297:nuc-nucalm-major-components-c.html)
