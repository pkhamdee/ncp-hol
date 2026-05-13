# Definitions Student

Before getting started with this lab, let's define some key terminology of business continuity and disaster recovery.

## What is a Recovery Point Objective (RPO)?

RPO is a measurement of the amount of data loss deemed acceptable by the business that an application or business process can have.

As an example, a business may say, "This application's RPO is no more than one hour". That means there needs to be protection in place (snapshots, backups, synchronous replication or application level High Availability) where you can restore while losing no more than data up to the maximum RPO of one hour.

Loss of less than one hour's worth of data would be acceptable. Loss of more than one hour's worth of data would be unacceptable.

## What is a Recovery Time Objective (RTO)?

RTO is the time it takes to recover from an outage. An RTO is usually broken down for restoring each application or business process, but may also include a maximum RTO for restoring all applications and services across the business.

## What is a Category?

A category is a construct in Prism Central that uses a key-value pair to label, classify, or categorize groups of VMs. You can think of them as tags or labs applied to entities. You can easily associate entities such as VMs, VGs, and CGs with a disaster recovery policy through categories.

The policy definition references the category. All of the entities are selected into the policy because they are appropriately categorized.

## What is a Protection Policy?

A Protection Policy is the construct for defining the Recovery Point Objective (RPO) as well as the location or locations to replicate snapshots.

## What is a Recovery Plan?

A Recovery Plan is the construct we use for defining how protected entities should be recovered. This includes boot stages, script execution and network mapping.

## What is Entity-Centric DR or Nutanix Disaster Recovery?

Entity-centric DR or Nutanix Disaster Recovery are both terms used to describe DR management on Virtual Machines (VMs), Volume Groups (VGs), and Consistency Groups (CGs) from Prism Central consisting of runbook automation, boot stages, and network mapping. Because the policies are applied to individual "entities" through categories, this is called entity-centric. This is in contrast to operations that would act on an entire storage container.

## Next Steps

Now that you understand key terms in Nutanix Disaster Recovery, let's move on to the next section.

[← Back: Overview](edge-lab-scenario3-overview.md) | [Home](edge-getting-started.md) | [Next: Setup →](edge-lab-scenario3-setup.md)