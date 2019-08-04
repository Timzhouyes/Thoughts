---
layout:     post   				    # 使用的布局（不需要改）
title:      Salesforce Trailhead				# 标题 
subtitle:   Walk through the 18 hours tutorial #副标题
date:       2019-08-03 				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - Salesforce
    - Web
---

Here is the note for Salesforce Basics tutorial on Trailhead.

# Get Started with the Salesforce Platform

## What does Salesforce Platform offer?

1. Develop custom data models and applications for desktop and mobile.
2. Heroku platform which let developers build scalable web apps and backend services by several languages.
3. Salesforce APIs, which let developers integrate and connect their enterprise data.
4. Mobile SDK.
5. so on 

# Develop without code

## What is metadata

In short, metadata forms structure of your organization.

# Code with Salesforce Languages

There are three core programmatic technologies to learn about as a Salesforce developer.

- Lightning Component Framework: A UI development framework similar to AngularJS or React.
- Apex: Salesforce’s proprietary programming language with Java-like syntax.
- Visualforce: A markup language that lets you create custom Salesforce pages with code that looks a lot like HTML, and optionally can use a powerful combination of Apex and JavaScript.



The difference between Lightning components and Visualforce pages.:

1. The primary difference is right in the name. With Lightning components, you’re developing components that can be pieced together to create pages. With Visualforce, you’re developing entire pages at once. 
2. While Lightning components are newer and better for things like mobile development, there are several situations where it can make more sense to use Visualforce.

# Extend the Salesforce Platform

## Get to Know the Lightning Platform APIs

The` __c` denotes that the object is a custom object or field. This query is using the automatically created API access point for the Property object to retrieve information about properties in your org.

##  Unleash Your Apps with Heroku

Heroku is a web development **platform** that lets you quickly build, deploy, and scale web apps.

# Import Data

## Introduction to Data Import

Salesforce offers two main methods for importing data.

- **Data Import Wizard**—this tool, accessible through the Setup menu, lets you import data in common standard objects, such as contacts, leads, accounts, as well as data in custom objects. It can import up to 50,000 records at a time. It provides a simple interface to specify the configuration parameters, data sources, and the field mappings that map the field names in your import file with the field names in Salesforce.
- **Data Loader**—this is a client application that can import up to five million records at a time, of any data type, either from files or a database connection. It can be operated either through the user interface or the command line. In the latter case, you need to specify data sources, field mappings, and other parameters via configuration files. This makes it possible to automate the import process, using API calls.

Here are different situations for different tools:

### Use the Data Import Wizard When:

- You need to load less than 50,000 records.
- The objects you need to import are supported by the wizard.
- You don’t need the import process to be automated.

### Use Data Loader When:

- You need to load 50,000 to five million records. If you need to load more than 5 million records, we recommend you work with a Salesforce partner or visit the [AppExchange](http://appexchange.salesforce.com/) for a suitable partner product.
- You need to load into an object that is not supported by the Data Import Wizard.
- You want to schedule regular data loads, such as nightly imports.

## Add Field Access with a Permission Set

Remember, a permission set is for *expanding* a user’s access to fields that are restricted in their profile.
