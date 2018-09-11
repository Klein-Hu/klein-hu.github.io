---
layout: post
title: Lessons Learned — A Year Of Going “Fully Serverless” In Production
categories: [Reading]
tags: [Career, Architecture]
fullview: false
comments: true
---

原文链接：[Lessons Learned — A Year Of Going “Fully Serverless” In Production](https://hackernoon.com/lessons-learned-a-year-of-going-fully-serverless-in-production-3d7e0d72213f)

**不用Severless，只能Sleepless**。整篇文章所述app用NodeJS写成。

应用分类：

1. 静态网站
2. 后台jobs
3. API Server

## 静态网页

别建网站，直接用CDN！作者推荐[Netlify](https://www.netlify.com/)

## API Server

直接上FaaS(Function as a Service)。作者推荐[AWS Lambda](https://aws.amazon.com/lambda/)。另外推荐[apex/up](https://github.com/apex/up)用在AWS Lambda上。apex/up用Go写的, 提供HTTP server。

### Packing for Serverless

作者为了apex/up写了[lambdapack](https://www.npmjs.com/package/lambdapack)。这里是[GitHub Repo](https://github.com/toriihq/lambdapack)

### Deployment

每个deployment不会覆盖前面的。基本上每个deployment系统都是如此。

### 本地开发/测试

本文没有好的解决方案，因为保证环境跟AWS一样比较困难。不如直接用AWS上的Test环境来得直接。

## 后台Jobs

同样推荐FaaS，作者推荐[AWS CloudWatch](https://aws.amazon.com/cloudwatch/)。另外推荐[apex/apex](https://github.com/apex/apex)用在AWS CloudWatch上。apex/apex也是用Go写的。

## Logging

起一个Lambda，里面跑[papertrail](https://papertrailapp.com/)可以作为初级解决方案。高级做法是再加一个[cloudwatch-to-papertrail](https://github.com/apiaryio/cloudwatch-to-papertrail)

## 环境变量和secret

显然不能check-in到代码里。作者推荐的办法是用[AWS ParemeterStore](https://aws.amazon.com/systems-manager/features/#Parameter_Store)。类似etcd。

## 冷启动

Lambda在一段时间不用之后就会“冷却”，之后启动需要600-2000ms。目前没有好的优化办法。一个workaround是定时call一个lambda，让这个lambda保持warmup的状态。

## 不要并行！

在AWS里，并行是API Gateway来调用多个Lambda来实现的。在一个Lambda里面只处理一个request。

## 结论

希望用Serverless实现DevOps的NoOps。不过这个只能是某些情况下可以适用。