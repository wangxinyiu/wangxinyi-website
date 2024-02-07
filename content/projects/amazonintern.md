---
title: A Server Migration to AWS (Internship at Amazon)
type: page
---

**Trasanction Stroage & Config Team**

# What I did
  1. Evaluated different AWS services for server deployment, including EC2, ECS, and Step Functions, and analyzed them against our current needs. Ultimately, I opted for Step Functions for implementation and carried out the system design accordingly.
  2. Decoupled the original code into smaller units and transitioned from Python to Java. Using the code-driven AWS Cloud Development Kit (CDK) approach, we configured various AWS services including Lambda functions, S3, VPC, among others.
  3. Used the AWS Step Functions service to orchestrate a series of independent AWS Lambda functions into a cohesive workflow, and addressed the challenge of passing parameters between Lambdas using an S3 bucket.
  4. Implemented parallel operations in stepfunction that allow for moving data from multiple source host to multiple target hosts.

## Result
![flow chart in step function1](/images/flowchart1.png)
![flow chart in step fucntion2](/images/flowchart2.png)

## Introdunction

Achieve the workflow(BatchMoveEndpoint) in AWS and demonstrate the server(SableManagementServer) can be implemented in Native AWS.

## Motivation

The current Server uses architecture which has the limitation on scalability and maintainability. Here are some reasons that are prompting move to AWS.

1. Improved maintainability:
Time required to maintain system, software, hardware update manual.

2. Enhanced scalability:
The server needs to handle the storgae of data and workflows efficientyly, but current architeture, a cluster of three hosts, has limitation. The inability to replace or modify individual hosts during a failure is a problem.

3. AWS benefits:
   - More easy to maintain: don't need to reply on our own data and infrastructure. Move to AWS allows all infrastructure management to be code-driven, preventing any conflitcts between deployment and maintenance.
   - More friendly
     - Lower Learning Curve
     - Extensive Documentation

## High-level Design
![highlevel_design](/images/lowlevel.png)
