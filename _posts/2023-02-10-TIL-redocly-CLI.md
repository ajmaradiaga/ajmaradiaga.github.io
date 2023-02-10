---
layout: post
title: TIL - redocly CLI to validate OpenAPI specification documents
---

I'm a big fan of the tools that are available on the [https://openapi.tools/](https://openapi.tools/) website. I've written about [some of them in the past](https://blogs.sap.com/2022/10/10/openapis-in-the-sap-ecosystem/) and I will add a new one to my toolkit - the [@redocly/openapi-cli](https://redoc.ly/openapi-cli).

The redocly OpenAPI CLI allows you to run validations against an OpenAPI specification. In my case, I'm interested in the linting functionality that it provides. Why linting? When trying to create an API in SAP API Management, the application wasn't happy with the file that I was providing. Unfortunately, no error surfaced in the UI. The browser console only mentioned that there was a problem with the Swagger Parser. So, I got in touch with someone from the product team to help me troubleshoot what was wrong with the file. My colleague pointed out the problem, authentication wasn't properly defined in the OpenAPI spec document.

That got me thinking.... how can I validate that the file I'm trying to upload is correct? I went to the OpenAPI tools, checked out the [@redocly/openapi-cli](https://redoc.ly/openapi-cli), installed it and when I passed my OpenAPI spec, bingo!, it raises the error that my colleague pointed out. This is a tool that  I will be using in the future when trying to troubleshoot problems with OpenAPI specs that I'm working with. 

![redocly recording](./../images/redocly-lint.gif)

When I originally ran the command, more warnings were raised by the tool. That said, it is possible to change its behaviour by specifying the rules that you want to check in [the configuration file](https://redocly.com/docs/cli/configuration/). Below, you can see the rules that I changed their setting.

```yaml
rules:
  operation-operationId: off
  tag-description: off
```
