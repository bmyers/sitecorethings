---
title: "Form Post By Convention"
date: 2017-07-06T22:40:41-05:00
draft: false
keywords: "Sitecore, MVC, Conventions"
description: "Sitecore controller rendering action method convention based post when not using form helpers"
tags: ["Sitecore MVC", "Conventions"]
---

# Form Post by Convention

A colleague of mine called me over while debugging an issue the other day. We had a View Rendering which contained a form
<!--more-->

````
@using (Html.BeginRouteForm(Sitecore.Mvc.Configuration.MvcSettings.SitecoreRouteName, FormMethod.Post, new { @class = "form" }))
{
    @Html.Sitecore().FormHandler("Contact", "ContactForm")
    ....
}
````
No big surprises here as everything was working as expected. Submitting this form will POST to the ContactForm action method of the Contact controller. This is done with the FormHandler helper provided by Sitecore.

We were comparing this form with another properly working one that was in a Controller Rendering. This rendering contained a form

````
@using (Html.BeginRouteForm(Sitecore.Mvc.Configuration.MvcSettings.SitecoreRouteName, FormMethod.Post, new { @class = "form" }))
{
    ....
}
````

This one did not use the FormHandler helper but was still working. We initially thought it had to be working because this rendering happened to be on the Contact page so the action rendered by

```` 
Sitecore.Mvc.Configuration.MvcSettings.SitecoreRouteName
````

was the same route as the Contact controller. This was simple enough to test so we changed the page name and submitted the form. It still worked. This baffled us. There was nothing in the html that would tell Sitecore where to POST to.

After lots of debugging and head scratching I happened to notice that the Controller Rendering's Controller Action field value was ContactForm.

````
[HttpGet]
public ActionResult ContactForm()
{
    return View("ContactForm.cshtml"));
}
````

Then, as seen with the FormHandler in the View Rendering, the POST was the same name

````
[HttpPost]
public ActionResult ContactForm(ContactFormViewModel viewModel)
{
    ....
}
````

What we discovered is as long as the action methods for the rendering of the form (specified in the Controller Rendering) and the action of the form match then everything works. I hope to dig into how this actually works but it appears to be convention based somehow.

Hope this helps others that may run into this issue.