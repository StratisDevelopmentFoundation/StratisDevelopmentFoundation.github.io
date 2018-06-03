---
title:  "Use Swagger.io to generate API clients in multiple languages"
categories: inactive_bounty coding
post_importance: 1
bounty: 10
---
Earn {{ page.bounty }} STRAT by completing this task.
{: .notice--info}

# Use Swagger.io to generate API clients in multiple languages

Observe that on [swagger.io](https://editor.swagger.io/) there is an online editor that takes a swagger configuration file. It has links (under "generate client") to generate client software for the given API configuration.

So, for example, if a python developer wanted to use the Stratis Full Node as the backend for a python application, they would create some python code that calls the Stratis Full Node API, handles the web requests, auth, retries, errors, etc. However, using this swagger.io generated code, they could potentially avoid writing all this code and use an import statement to import the client, then do simplified calls to the client.

# Task Description

The task is:

* Locate the swagger config in the SFN
* Make sure that our version of swagger config can be pasted into the swagger.io editor
* Generate client software for a language, such as python
* Try using the client software to interact with the Stratis Full Node
* Report back to the SDF if everything worked properly, or if any changes are needed to better integrate this functionality