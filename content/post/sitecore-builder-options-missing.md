---
title: "Sitecore Builder Options Missing"
date: 2017-07-10T21:15:25-05:00
draft: false
keywords: "Sitecore, builder options, url rewrite"
description: "Sitecore builder options missing? Unable to add standard values or set base templates."
tags: ["Sitecore", "IIS", "Url Rewrite"]
---

# Builder Options Missing in Content Editor
When editing templates in Sitecore we are used to seeing the Builder Options tab appear when on the builder tab. This is useful for setting base templates and adding the Standard Values item. Recently I ran into an issue where the builder options was not showing up.

![Builder Options](/images/builder-options.png)

After many hours of debugging javascript, permissions, decompiling Sitecore's dlls and anything else I could think of I finally stumbled upon the issue. I couldn't reproduce this in my local environment, it only occurred on the production Content Management server. Turns out there was a URL Rewrite rule in IIS that was not in the local environment.

If you are having issues with the Builder Options showing, check any URL Rewrite rules. If the rule is needed add a check to make sure the url does not contain '/sitecore/'.