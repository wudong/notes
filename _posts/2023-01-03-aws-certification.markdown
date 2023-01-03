---
layout: post
title:  "AWS Solution Architect Certificate"
date:   2023-01-03 22:35:46 +0000
categories: aws certificate
---


# Compute 

## EC2
### AMI: 
- An AMI is an *image baseline* with an operating system and applications along with any *custom configuration*
- AWS Marketplace: online store that allows you to purchase AMI from trusted vendros.
- Cumminit AMIs: created and shared by other AWS members.

### EC2 Instance Types
- ECUs: EC2 Compute Units for the instance
- AES-NI: Advenced Encryption Standard - New Instructions, needed for enhanced data protection.
- AVX: for application focus on a/v, scientific calc, 3D modeling, etc.
- Mostly we focus on
  - vCPUs
  - Memory
  - Instance Storage (EBS)
  - Network Performance

### Instance Purchasing Options
- On-Demand: flat rate determined on the instance type, charged by second, typically used for *short-term* uses, workload cannot be interrupted.
- Reserved: saving as much as 75%, 1 or 3 years.
  - All upfront: complete payment for 1 or 3 years
  - Partial Upfront
  - No Upfront
- Scheduled: pay for the reservations on a recurring schedule, either daily, weekly, or monthly.
  - changed even if it is not used
- Spot: bid for a unused EC2 compute resources.
- On-Demand Capacity Reservations:
