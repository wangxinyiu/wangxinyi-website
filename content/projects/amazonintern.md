---
title: A Server Migration to AWS (Internship at Amazon)
type: page
---

**Trasanction Stroage & Config Team**
- [Introdunction](#introdunction)
- [What I did](#what-i-did)
- [Motivation](#motivation)
- [Result](#result)
- [High-level Design](#high-level-design)
- [Details](#details)
  - [Why Stepfunction rather than EC2?](#why-stepfunction-rather-than-ec2)
  - [Why do I plan to use AWS CDK instead of CloudFormation?](#why-do-i-plan-to-use-aws-cdk-instead-of-cloudformation)
  - [How do I make sure the code quality?](#how-do-i-make-sure-the-code-quality)

# Introdunction

Achieve the workflow(BatchMoveEndpoint) in AWS and demonstrate the server(SableManagementServer) can be implemented in Native AWS.        

[PDF: [Proof of Conceop] SableManagementServer BatchMoveEndpoints in NAWS: Design Doc](https://drive.google.com/file/d/1MczefSZ5z4I1cmAJyJ-hwkjFvr4U2FXz/view?usp=sharing)

# What I did
  <!-- 1. Evaluated different AWS services for server deployment, including EC2, ECS, and Step Functions, and analyzed them against our current needs. Ultimately, I opted for Step Functions for implementation and carried out the system design accordingly.
  2. Decoupled the original code into smaller units and transitioned from Python to Java. Using the code-driven AWS Cloud Development Kit (CDK) approach, we configured various AWS services including Lambda functions, S3, VPC, among others.
  3. Used the AWS Step Functions service to orchestrate a series of independent AWS Lambda functions into a cohesive workflow, and addressed the challenge of passing parameters between Lambdas using an S3 bucket.
  4. Implemented parallel operations in stepfunction that allow for moving data from multiple source host to multiple target hosts. -->

1. Migrated the BatchMoveEndpoint (BME) workflow to AWS, ensuring full implementation of SableManagementServer (SMS) within the native AWS environment, which reduced 128 maintenance tickets annually.
2. Evaluated different AWS services for server deployment, including EC2, ECS, and Step Functions, and analyzed them against our current needs. Ultimately, I opted for Step Functions for implementation and carried out the system design accordingly.
3. Decoupled the workflow into 12 modular units, transitioning from Python to Java, and rigorously tested each unit to ensure robust functionality, thereby achieving 99.9% uptime.
4. Utilized the AWS Cloud Development Kit (CDK) for a code-first infrastructure approach, configuring a suite of AWS services including Lambda functions, S3, and VPC, which resulted in a 75% reduction in deployment time compared to manual configuration via the AWS Management Console.
5. Orchestrated a network of independent AWS Lambda functions into a unified workflow using AWS Step Functions, overcoming the complexity of inter-lambda parameter passing via S3 bucket integration. This architecture enhancement increased process efficiency by 40% and reduced operational costs by 30%.
6. Enhanced data agility by implementing parallel operations within AWS Step Functions, enabling efficient data transfer across multiple source and target hosts.

# Motivation

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

# Result
![flow chart in step function1](/images/flowchart1.png)
![flow chart in step fucntion2](/images/flowchart2.png)

# High-level Design
![highlevel_design](/images/lowlevel.png)

# Details
## Why Stepfunction rather than EC2?
1. 自动扩展和容错：Step Functions 自动处理扩展和容错，确保工作流的执行即使在高负载下也能保持高可用性。相比之下，使用 EC2 实例通常需要手动设置自动扩展策略和容错机制。
    - 自动缩放：Step Functions 可以根据工作流执行的数量自动缩放，以适应请求量的变化。这意味着在请求量增加时，Step Functions 能够动态地增加资源来处理更多的工作流，并在负载减少时相应地减少资源。
    - 内置容错：Step Functions 通过多个可用区内的冗余部署来提高容错能力。如果一个区域发生故障，Step Functions 可以自动切换到其他区域以维持服务的持续可用性。
    - 状态持久性：Step Functions 保持每个工作流的状态，即使在高负载或部分系统故障的情况下，也能确保不丢失状态信息。这有助于在发生故障后快速恢复执行。
    - 错误处理和重试策略：Step Functions 允许定义错误处理逻辑和重试策略，这有助于在执行步骤失败时自动进行处理和重试，而无需人工干预。
    - 分布式执行：Step Functions 的设计允许工作流的分布式执行，工作流中的各个步骤可以在不同的计算资源上并行或独立运行，从而提高了整体的可用性和效率。
    - 持续监控和维护：AWS 持续监控 Step Functions 服务的性能，并自动进行维护更新以确保服务的最佳状态。
2. 成本效益：对于事件驱动或间歇性工作负载，Step Functions 可能更加成本效益，因为你只为工作流执行的状态转换付费，而不需要为持续运行的服务器付费。而 EC2 实例则需要按小时计费，不管它们是否处于活动状态。
3. 简化的开发和部署：Step Functions 提供了声明式的状态机模型，简化了工作流的定义、部署和管理。这可以加速开发过程，特别是对于复杂的业务逻辑和多步骤工作流。
4. 工作流管理：Step Functions 是一种完全托管的服务，专为简化复杂工作流的协调和状态管理而设计。如果你需要编排多个 AWS 服务（如 Lambda、S3、DynamoDB 等）之间的工作流，Step Functions 提供了一个可视化界面和声明式 API 来定义和管理这些步骤。
5. 无服务器架构：Step Functions 可以与 AWS Lambda 等无服务器计算服务无缝集成，使你能够构建无需管理服务器的应用程序。这种模式减轻了运维负担，因为 AWS 负责维护底层计算资源。
    -  无服务器架构的关键特点包括：
       -  事件驱动：大多数无服务器计算平台都是基于事件驱动的，这意味着你的代码会在检测到一个或多个预定义事件时自动执行。
       -  按需计算：代码只在需要时运行，你只为实际计算时间付费。当代码不运行时，没有费用产生。
       -  管理型服务：底层基础设施由云服务提供商管理。这包括服务器的维护、扩展和更新等任务。
       -  自动扩展：应用程序可以根据需要自动扩展到几乎无限的容量，也可以缩减到零。
       -  内置高可用性和容错：无服务器平台通常在多个数据中心内部署，以实现高可用性和容错性。
6.  集成和API支持：Step Functions 提供广泛的 API 支持和与其他 AWS 服务的内置集成，这使得构建和集成跨服务的复杂应用程序变得更加容易。

## Why do I plan to use AWS CDK instead of CloudFormation? 
AWS CloudFormation and AWS CDK are automation tools provided by AWS for creating and managing cloud environments.       

CloudFormation offers a stable, straightforward, and easily understandable way to define and deploy cloud environments, but may become complex and hard to maintain when dealing with complex environments.      

On the contrary, CDK allows you to define your cloud environment using regular programming languages such as TypeScript, Python, Java, etc., making code reuse and abstractions simpler, but its learning curve may be steeper, especially for those unfamiliar with programming.

Therefore, if you value the advantages of a programming language, like code reuse and higher-level abstractions, along with using familiar development tools, choosing AWS CDK over CloudFormation makes sense.

## How do I make sure the code quality?
1. Compile-time check
   - Use type-safe parameters and return types to avoid type conversion errors.
2. Unit Testing
   - Use testing frameworks such as JUnit to automate the tests.
   - Check boundary conditions and exceptions.
3. Integration test
   - Test functions in the broader context of the system to ensure that interactions with other components work as expected.
4. Code review
   - Check for potential logic errors and suggestions for improvement through code reviews by colleagues.
5. Static code analysis
   - Use tools such as SonarQube, Checkstyle, PMD, or FindBugs to detect potential issues and bad practices in your code.
6. Code coverage analysis
   -  Use a tool such as Jacoco to analyze code coverage and ensure that the test cases cover all code paths.
7. Continuous Integration (CI)
   -  Automate the above tests in your CI/CD pipeline to ensure that every commit or merge request doesn't break existing functionality.
8. Error handling and logging
   - Make sure that your function has an appropriate error handling mechanism.  
   - Use logging to track the behavior of your functions for easy diagnosis and debugging of problems.