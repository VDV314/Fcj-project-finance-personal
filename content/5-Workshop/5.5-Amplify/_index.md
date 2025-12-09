---
title: "Deploy Frontend with Amplify"
date: "`r Sys.Date()`" 
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---



In this section, you will deploy the Finance Tracker React application to AWS Amplify, which provides a complete hosting solution with CI/CD capabilities.

## Overview

AWS Amplify will:
- Host your React frontend application
- Provide automatic builds on code commits
- Enable HTTPS by default
- Offer a custom domain (optional)
- Provide CI/CD pipeline from GitHub

## Prerequisites

Before starting, ensure you have:
- A GitHub account
- The Finance Tracker repository forked to your account
- Repository URL: `https://github.com/VDV314/financeTracker`

## Steps to Deploy with Amplify

### 1. Navigate to AWS Amplify Console

1. Open the AWS Management Console
2. Search for **AWS Amplify** in the services search bar
3. Click on **AWS Amplify**
4. Click **Create new app**

![Start Building](/images/5-Workshop/5.5-Amplify/Create-new-app.png)

### 2. Choose Source Code Provider

On the **Start building with Amplify** page:

1. Under **Deploy your app**, select **GitHub**
2. Click **Next**

![Choose GitHub](/images/5-Workshop/5.5-Amplify/buildAmplify.png)

### 3. Add Repository and Branch

1. In the search box, enter: `VDV314/financeTracker`
2. Select your repository from the dropdown
3. For **Branch**, select `main`
4. Click **Next**

![Add Repository](/images/5-Workshop/5.5-Amplify/add-repository.png)

### 4. Configure App Settings

On the **App settings** page:

1. **App name**: Enter `financeTracker`
2. **Build settings**: Amplify will auto-detect your framework

The build settings should show:
- **Framework**: None (or React if detected)
- **Frontend build command**: (leave empty or auto-detected)
- **Build output directory**: `/` (root directory)

![App Settings](/images/5-Workshop/5.5-Amplify/app-settings.png)
3. Click **Next**

### 5. Review and Deploy

1. Review all your settings:
   - **Repository details**: github / VDV314/financeTracker
   - **Branch**: main
   - **App settings**: financeTracker
   - **Framework**: None
   - **Advanced settings**: Standard build, default image

![Review](/images/5-Workshop/5.5-Amplify/review.png)

2. Click **Save and deploy**

### 7. financeTracker Overview
In the Amplify Console overview, you can see:

**Get to production section**, with options to:

1. **Add a custom domain** : Use your own domain with HTTPS

2. **Enable firewall protections** : Use AWS WAF for enhanced security

3. **Connect new branches** : Set up multiple environments

**Branches section** displays:

-The **main branch** (Production branch):

-**Domain** : The Amplify-provided URL for your app

-**Last deployment** : The timestamp of the latest deployment

-**Last commit** : The latest Git commit message
![Overview](/images/5-Workshop/5.5-Amplify/Overview.png)


