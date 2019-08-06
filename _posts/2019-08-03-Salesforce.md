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

# Use Formula Fields

## The Formula Editor in Action

### Example 1: Displaying an Account Field on the Contact Detail Page

### Example 2: Displaying the Number of Days Until an Opportunity Closes on a Report

In this part, the most important part is to use `TODAY` to get the value of today.

### Example 3: Finding Distinct Objects Using the Power of One

It is used to figure out the number of distinct items, such as there are only 6 users but 100 records related, `Power of One` can do the distinction.

To write this formula, create a custom formula field on the User object. Name it Unique Users, give it a Number return type, and select 0 from the Decimal Places drop-down list. Click **Next** to open the formula editor. For this formula, you don’t need to insert any fields, operators, or functions. Instead, enter the number 1.

Yes,just the number 1.

![The Power of One formula.](../img/345cb6052afab5fdfbcab44958df3d46_formulas_one.png)

## Debugging Formulas

So we can also check the formula online. Such as :

-  **Missing parentheses**: This error most often occurs when the number of opening parentheses doesn’t match the number of closing parentheses. It can be particularly difficult to avoid this error if you’re using several functions at once. Try breaking your function into multiple lines so it’s easier to tell which sets of parentheses belong together.

![A formula that's missing a parenthesis.](../img/1c7cf053bc8a8a8e35441bed5fed42f4_formulas_missing_paren.png)

- **Incorrect parameter type**: If you give a function a number parameter when it expects text (or any other combination of data types), this is the error you’ll see. Always check the help text or the documentation so you know what kind of parameters a function accepts.

![A formula with an incorrect parameter type.](../img/62e5028d414dd905f4deabc99fbfc925_formulas_incorrect_param.png)

- **Incorrect number of parameters for function**: If you input too many or too few parameters into a function, the syntax checker alerts you. Again, check the help text or documentation for guidelines on inputting parameters to specific functions.

![A formula with too many parameters.](../img/41d384fba9ed9ed93d44fb714247920b_formulas_incorrect_num_params.png)

- **Formula result is incompatible with formula return type**: You’ll see this error if you select one data type when creating the formula field but write a formula that returns a different data type. In the example below, you can see that My Account Formula expects to return a number (shown in parentheses next to the formula name), but the TODAY() function returns a date. The error tells you what the expected data type is, but you can always reference the documentation beforehand to avoid the error.

![A formula that returns a result of the incorrect data type.](../img/28195bf4a64481276a7ef8624c1a83e4_formulas_incompatible_type.png)

- **Field does not exist**: This error indicates that you’ve included a field in your formula that your object doesn’t support. In this case, check your spelling and capitalization. If you can’t find any mistakes, try inserting the field from the **Insert Field** menu again to make sure you’re referencing it correctly.

![A formula with a misspelled field name.](../img/b13d463398c3259a2b611121992cc6af_formulas_field_doesnt_exist.png)

- **Unknown function**: In this case, check that Salesforce supports the functions you’re using. You’ll also get this error for misspelled functions.

![A formula that includes an unsupported function.](../img/f548b71771f8a6e3eb37a2182b1a0171_formulas_unknown_function.png)