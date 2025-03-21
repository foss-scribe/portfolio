+++
title = "Portfolio"
description = "A portfolio of professional technical writing work created by Chris Gardiner-Bill"
date = "2023-01-01"
author = "Chris Gardiner-Bill"
+++

Included in this portfolio is a broad collection of samples with a description of why and how they were created. They are listed by company. I have also linked to a selection of informal blog posts of a technical nature.

This page is work-in-progress and subject to change.

### Canva

#### [Autofill guide](https://www.canva.dev/docs/connect/autofill-guide/)

A guide on how use Canva Autofill API to inject data into dynamic designs, created during my secondment to the Ecosystem team as part of the Connect GA project.

`Markdown` `NextJS` `API`

#### [Return navigation guide](https://www.canva.dev/docs/connect/return-navigation-guide/)

A guide on using Canva's return navigation flow to let users move between third-party integrations and Canva's editor, created during my secondment to the Ecosystem team as part of the Connect GA project.

`Markdown` `NextJS` `API` `Integration`

### Specialized Bicycle Components

#### [WeChat Authentication Guide](/examples/sbc_auth_we_chat/)

A guide on how to authenticate with Specialized's Identity Service using WeChat. To write this guide, I worked closely with the Java developer responsible for creating the workflow, as well as assisting with testing using Postman and WeChat on Android. This guide was included as part of a catalogue of recipes contained within an MkDocs project and deployed to the Specialized Admin Portal.

`Markdown` `MkDocs` `WeChat` `Authentication`

#### [Login API Endpoint Documentation](/examples/sbc_auth_login)

A detailed guide to using the login API endpoint for Specialized's Identity Service. Originally included in a page documenting all authenticated-related endpoints in the Identity Service. The source was written as a standalone, re-usable document that could be called using a Jinja2 include directive. This document was of an MkDocs project and deployed to the Specialized Admin Portal. All endpoint documentation in the suite followed this pattern.

*Note: this document has been altered from the original format (MkDocs Python-flavoured Markdown) for compatibility with Hugo. Sections have been converted from tabs to headings.*

`Markdown` `MkDocs` `API` `Authentication` `REST` `cURL` `JSON`


#### Other work (available on request)

* How to integrate Zone5Cloud with Pipedream.io and Shopify
* How to build an athlete leaderboard
* Coding:
    * OpenAPI JSON to MkDocs-compatible markdown convert using Python.
    * Zone5Cloud integration module created as a starter project for [Purple Patch Fitness](https://www.purplepatchfitness.com) using Python.
    * Serverless e-commerce store created for coach and YouTuber [Dylan Johnson](https://www.dylanjohnsontraining.com)

### Equifax Australia

#### User guide: Managing HEM in DecisionPoint 3

A user guide demonstrating how to convert and upload Household Expenditure Measure (HEM) data into DecisionPoint 3. By following this guide, clients were to manage their quarterly HEM updates and use the data in their credit application solutions without relying on Equifax's delivery team.

The document was produced using Asciidoctor and managed as part of a single Git repository containing over a hundred and fifty documents and thousands of re-usable document fragments. From the asciidoc source, the manual was published to [HTML](/files/dp3_hem/dp3_hem.html) and [PDF](/files/dp3_hem/dp3_hem.pdf) formats.

As with all documents in this suite, the HEM guide was assembled from multiple fragments and includes conditional logic which enabled the production of multiple versions containing client- and release-specific content. To build these versions, I created a Ruby script that automatically generated each version as HTML then converted the resulting files to PDF using PrinceXML.

`Asciidoctor` `User Guide` `PrinceXML`


### Writing/Recruitment tests

Writing exercises undertaken as part of job applications.

#### [Swagger Pet Store API](/examples/swagger_petstore/)

Brief: The task was to create a tutorial that explains how to: 

* Fetch a list of pets from the Swagger Petstore API - https://petstore.swagger.io/. 
* Render the results in a web page. 

The complete exercise is [available on my GitHub account](https://github.com/foss-scribe/swagger-petstore-api-guide-tutorial).

`Markdown` `JavaScript` `API Documentation`

### Blog posts

I regularly publish technical articles on Medium and this site. While informal in tone (and not rigorously edited or proof-read), they demonstrate my writing across a variety of topics and interests.

* [Installing backstage.io on macOS (for technical writers)](https://chris.gardiner-bill.com/posts/backstage_macos/)
* [Optimising OBS on a 2020 MacBook Pro](https://technicalprose.medium.com/optimising-obs-on-a-2020-macbook-pro-aca7bbbe55e9)
* [Managing storage on a cheap Linux VPS](https://technicalprose.medium.com/managing-storage-on-a-cheap-linux-vps-f576e6ac4d3c)
* [Using SOX on macOS](https://technicalprose.medium.com/using-sox-on-macos-48f25014d1e3)
* [My favourite markdown tools](https://technicalprose.medium.com/my-favourite-markdown-tools-fc1cdc537e67)
* [How to seek and destroy filter words with Scrivener](https://technicalprose.medium.com/how-to-seek-and-destroy-filter-words-with-scrivener-a968dd8cf914)
* [How to host multiple local websites with macOS Big Sur](https://technicalprose.medium.com/how-to-host-multiple-local-websites-with-macos-big-sur-72d947f3e97c)