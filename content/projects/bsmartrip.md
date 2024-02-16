---
title: A travel planning app based on Google Maps and TripAdvisor
type: page
---
[Presentation Slides Link](https://docs.google.com/presentation/d/1r6r_yNDs9v0hbEkXNEE-z0oToEigMs-DRg6-qItkUzc/edit?usp=sharing)

**Catalogs**
- [Introdunction](#introdunction)
- [STAR1 -\> Rapid API](#star1---rapid-api)
- [Star2 -\> Priority classfication](#star2---priority-classfication)
- [Details](#details)
  - [后端开发文档](#后端开发文档)
  - [RestAPI](#restapi)
  - [ER diagram](#er-diagram)
  - [Flow Chart](#flow-chart)
- [Design Picture](#design-picture)

# Introdunction
在我的前一段工作中，我担任后端API开发的关键角色，负责实现和维护应用程序的核心后端功能。这包括但不限于用户注册、认证以及数据检索和管理等多个方面。   

1. 在用户注册方面，我主要负责使用Spring Security框架来实现基于Token的安全系统。通过这个框架，我创建了一个自定义的认证流程，它能够在用户注册时生成一个安全的Token，并在用户登录时进行验证。这个Token是通过安全算法加密的，并且每次请求都需要携带它来确保用户身份的合法性。使用Spring Security使得整个认证过程不仅安全，而且可维护性和扩展性也非常好，为应用提供了坚固的安全基础。

2. 我还负责集成了第三方API，如RapidAPI中的Travel Advisor，为规划合理路线功能提供数据支持。这项工作不仅需要理解外部API的工作原理，还需要处理数据同步和异常管理，以确保用户能够获取到准确和及时的旅行建议。
3. 至于数据的搜索和下载功能，我使用了elastic search去构建，其可以允许用户根据多个维度进行筛选，使得数据检索既高效又精确。

在这一过程中，我与前端团队紧密合作，确保了API设计的前后端一致性，以及响应性能的优化。通过不断的沟通和迭代，我们共同提升了整个应用程序的稳定性和用户体验。

这些工作的成果不仅体现在直接的用户反馈上，也在于后端服务的稳定性和可扩展性方面。我为能够支持团队并助力前端功能的实现和业务需求的扩展而感到自豪。


# STAR1 -> Rapid API
1. **情境 (Situation)**        
在开发一个需要大量旅游信息的项目初期，我们计划使用TripAdvisor的API来获取数据。这个API的成本是平均每个月$350，但它的rate limit是每小时1000次请求，这对于我们的需求来说既昂贵又效率低下。

1. **任务 (Task)**     
鉴于成本和效率的双重挑战，我的任务是找到一个更经济高效的解决方案，以支持我们的项目需求，同时不牺牲所需的数据质量和访问频率。

1. **行动 (Action)**       
为了解决这个问题，我开始寻找替代的API服务。通过广泛的市场调研，我在Rapid API平台上找到了一个名为"Travel Advisor"的API。它提供了更符合我们预算和性能需求的服务条件：免费支持每月500次请求，同时其rate limit为每秒5次请求。我对比了两个API提供的服务内容、数据质量和使用条款，确保"Travel Advisor"能满足我们的项目需求。

1. **结果 (Result)**
选择"Travel Advisor"API后，我们的团队在接下来的三个月内节省了$1050的成本，同时获得了更高效的数据访问能力。这个决策不仅优化了我们的项目预算，还提高了开发效率，确保项目能够在资源有限的情况下顺利进行。

    通过这个经历，我学会了如何有效地评估技术解决方案的成本效益，并且能够灵活调整计划以适应项目需求和预算限制。这个经验也强化了我的研究能力和决策制定能力。


# Star2 -> Priority classfication
**Situation**: I used to lead a team to finish a full-stack web development within one month. The time is very limited in this situation.

**Task**:  I have been given a big idea about this application which need to help our customer to provide travel planner behind they pick a city where they want to go. According to the time and the workload. It’s really not an easy thing.

**Action**: 

1. At the first, We discussed and brainstormed at the first meeting and the most important thing to finish our project quickly and high-quality is that I need to prioritize our functions. like, I separate our functions into different levels, like P0, P1, and P2.
    1. P0 function is the basic function of our project. like, log in/ log out, Show points of interest in the map, and give a rational rounte for users, etc. I need to finish these basic functions at least in that month. 
    2. P1 is the functions I need to try my best to finish like user authentication, selectively sharing daily plans, etc. → authentication is for user can log in by their indentities and we use the authentication function to provide account safe.
    3. P2 is I will achieve it in the future. like, rate other users' trips, Invite friends to co-edit, etc.
2. Next, I had done many references for one of the most popular industrial modes, which is the agile development mode for achieving more efp rficiency. I separate our group into two sub-group, like front-end and back-end.
3. Third, We meet four times a week, in order to put forward the feedback from each group member and so enhance the performance of our functional implementations.

**Result**: I finish all functions of P0 and P1 with high standards and quality, even remnant four days. Then I could spend some time summarizing the whole project and write a good document. I deployed my web application to amazon EC2 which has improved the performance in visibility and stability by over 18.2%. By the way, we have been praised by our group manager.

I made a short viedo for my manager to introduce my whole project detail. 

The high level to summarize how to work under a time limit for me is to plan each day carefully.
    
# Details
Make your trip visible and customize your daily travel routes.

## 后端开发文档
[Backend Documentation](https://docs.google.com/document/d/1dKlzNrTD2t1VIjt8ihFopkxEANhsD0Awlyb0B7oyjRk/edit?usp=sharing)

## RestAPI
**1.Registration & authentication**
```java
	@PostMapping("/register")
    	void addUser(@RequestBody User user)

	@PostMapping("/authenticate")
    	public Token authenticateUser(@RequestBody User user)
```

**2.Search points**
```java
@GetMapping(value=”/search”)
List<Point> searchPoints(@RequestParam category, required = false)
  if (category != null) {return points in that category}
  else {return all points} 
  // Backend will store all the points in the database first and then return the points.

```

**3. Save a plan**
```java
@PostMapping(value=”/plan)
void savePlan(@Requestbody requestbody, Principal principal)
// requestbody includes: 
  note of the plan
  date of each daily_plan
  list of points of each daily_plan
// principal
  requires frontend to send a token for authentication, like RestAPI in StayController in project staybooking
```

**4. Delete a plan**
```java
@DeleteMapping(value=”/plan/{planId}”)
	void deletePlan(@PathVariable Long planId, Principal principal)
	//requires frontend to send a token for authentication, like RestAPI in StayController in project staybooking
```

**5. Get plans by user**
```java
@GetMapping(value=”/plan”)
	List<Plan> getPlansByUser(Principal principal)
    // principal
    requires frontend to send a token for authentication, like RestAPI in StayController in project staybooking

```

> requires frontend to send a token for authentication

## ER diagram
![alt text](/images/SmartTripERdiagram.png)

## Flow Chart
![alt text](/images/SmartTrip.png)

Travel Advisor from rapid API: https://rapidapi.com/apidojo/api/travel-advisor


# Design Picture
![1](/images/Travel_Planner_1.JPG)
![2](/images/Travel_Planner_2.JPG)