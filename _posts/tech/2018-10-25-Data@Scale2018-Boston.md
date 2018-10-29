---
layout: post
category : 技术笔记
tagline: "Supporting tagline"
tags [dev]:
---

## Deleting Data @ Scale, by Facebook

**Use case**: Facebook needs to delete specific data for better data maintain and control

**Problems**: Data in facebook is graph like. And facebook will remove all the related data from DB. Which cannot happen during a web request

**Solution**: 
* For web request, only to shallow delete. Which hide it from UI. And using a Queue system to handle the background deep delete
* Use the Facebook internal schema to defined the delete logic between objects
* Design an auto detect system to make sure there are no orphan items. 
* Design a back up system so that within a certain period of time, could revert any unwanted deletion. 

## Sampling to Reduce Data Warehouse Resource Consumption, by Facebook

Only calculate using 1% of the data in warehouse may result in the same graph data set at the end, which would significantly reduce the resource usage. 

User could define a given bare rate so that this tool could automatically chose the right sample size. 

## Kubeflow: Portable Machine Learning on Kubernetes, by Google

* Google invest a lot on Kubernetes (k8s), which could be something we could relay on later as the container orchestration tool
* Kubeflow to setup the machine learning infrastructure. Which allow developer to only create ML code

## How DataXu Built a Cloud-Native Warehouse, by DataXu

* Use resource on-demand service if possible.

## Building Highly Reliable Data Pipelines, by Datadog

* Split on Horizontal (by context) and Vertical (multiple steps) to improve performance and make it fail resilient.