---
title: "Sitecore 9 Upgrade From 8.2"
date: 2017-11-22T09:31:46-06:00
draft: true
keywords: ""
description: "Upgrading Sitecore 8.2 to 9.0"
tags: ["Sitecore", "Upgrade", "9.0"]
---

# Setup
1. Download all prerequisites from Upgrade guide
1. Restore backups of all databases (Core, Master, Web, Analytics)
1. Fix SQL Server user link EXEC sp_change_users_login 'Auto_Fix', 'SitecoreAdmin' on each restored database
1. Verify site running against restored databases (verify connectionstrings.config)
1. Stop all active running tests in Experience Optimization
1. Since we are using xDB, we had to do 3.1.2 and 3.1.3
1. 3.1.2 didn't have any items
1. 3.1.3 validation came up with 2 items with duplicate names, resolved those
1. Upgrade SPE to 4.7

# Upgrade
1. 3.1.4 - Disable xDB, WFFM
1. Run all 3 sql scripts
1. Deploy dacpac (ExperienceForms, Sitecore.Processing.Tasks)
1. Install Sitecore Update Installation Wizard 3.1.0 rev. 170919.zip
1. Recieved error right away "Method not found: '!!0[] System.Array.Empty()'". Install .NET 4.6.2
1. Analyze .update package (review warnings, Custom code should be recompiled - 1313 - mostly SPE)
1. Updgrade SPE
1. Analyze again (now 245)
1. Config file changes - SiteDefinition.config, ExperienceAnalytics/Sitecore.ExperienceAnalytics.ReAggregation.Scheduling.config - changed hour for debugging locally, skip
1. Install Update

# Code Updates
1. Reference new Sitecore.* dlls
1. Upgrade Glass Mapper to 4.4.1.331-beta (non-beta version currently doesn't work with 9)

# Module Updates
1. WFFM - run .update
1. SPE - we were on an older version, installed 4.7

# Install and Configure xConnect
1. Install using PowerShell script
1. Update connection strings with correct databases, user ids, passwords, FindByValue (thumbprint)
1. Grant access to certificate for site app pool
1. Troubleshoot any security issues via Kam's blog post - https://kamsar.net/index.php/2017/10/All-about-xConnect-Security/
1. New Query, SQLCMD mode, execute post install to add collectionuser to dbs
1. 

# Configure Solr
1. Already had Solr installed via PowerShell script
1. Change settings for adding cores to new instance
1. Control Panel > Indexing > Populate Solr Managed Schema
1. Update old Lucene configs to Solr configs

# Migrate xDB data
1. Install packages
1. Run migration tool

# Post
1. Rebuild Link Databases
1. Deploy Marketing Definitions
1. Re-index

# Sitecore Forms
1. Broken - Remove Broken Links, Rebuild Links Database... not fixed
1. Re-run update package
1. (╯°□°）╯︵ ┻━┻)
1. Stand up XP0 using SIF, serialize core:/sitecore/client/Business Component Library/version 2, revert tree on upgraded instance

