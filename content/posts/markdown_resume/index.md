+++
title = " Writing a resume with Git and markdown"
date = "2023-01-21"
description = "A tutorial on how to write a resume with git and markdown"
author = "Chris Gardiner-Bill"
tags  = ["tutorial"]
+++


After being made redundant, I needed to write a resume for the first time in ten years. Since 2014 every position I held was thanks to head-hunting by a recruiter, based on my LinkedIn profile, and I skipped straight to the interview stage. 2023 is an unfamiliar road, and we seem to be amid a severe contraction in the tech sector.

The last time I wrote a resume, I used either Apple Pages or Microsoft Word — I can't remember which. Since then, I've spent the last ten years of my career writing *docs as code* I figured it was time I wrote my resume in Markdown and used Git for version control.

## Markdown

For my resume, I use as close to vanilla markdown as possible, so I can process it using any compiler. While I use MultiMarkdown on my Mac, keeping it vanilla means the content complies as expected with Pandoc, GitHub and Hugo.

The body of my resume uses only headings, paragraphs, lists, and code blocks. I also use a front-matter block (more on this later), which most markdown flavours and compilers support.

Here's an excerpt of my current resume, but you can find the [full version in this portfolio's source code on GitHub](https://github.com/foss-scribe/portfolio/blob/master/content/resume.md), albeit with the front matter converted to TOML for better compatibility with Hugo.

"`md
---
Title: Chris Gardiner-Bill Resume
Author: Chris Gardiner-Bill
Date: 2023-01-20
css: ../styles.css
---

# Chris Gardiner-Bill

[Email](mailto:me@example.com) | [LinkedIn](https://www.linkedin.com/in/chris-gardiner-bill/) | [GitHub](https://github.com/foss-scribe)

## Profile

A senior technical writer and trainer with an exceptional blend of documentation and technical abilities. Significant experience producing high-quality documentation and training material for technical, business and generalist audiences across APAC and the EU.

Broad industry participation including manufacturing, banking and financial services, engineering services, software development, sports tech, analytics and publishing sectors. A specialist in *docs as code* using Git, Markdown, AsciiDoc, static site generators, and CI/CD to automate and rapidly publish docs to market at scale, while minimising the costs of production and improving quality.

## Key capabilities

* Technical Documentation
* Technical Writing
* Editing
* Docs as Code
* Training
* Instructional Design
* Proposal Writing
* Problem Solving
* Automation
* API Testing
* Project Management
```

As the excerpt shows, it's all vanilla Markdown — clear and easy to write. I drafted mine in VS Code, but you can choose from many great markdown editors, some of which enhance the writing experience more than a code editor.

## Styling

One of the advantages of Markdown over a word processor, like Microsoft Word, is the separation of content from presentation. Markdown lets you focus on your words, not waste time with fonts, colours, and layout.

Markdown is formatted when compiled by a processor, like those I mentioned above. When I build my resume with Hugo, [the resulting HTML](https://chris.gardiner-bill.com/resume/) is styled according to the theme, and no additional work is required.

However, when I make a resume for a specific job application (more on that below), I need a more conservative design that adopts the look of a traditional resume and is print-friendly.

However, when I build the document on the command line with MultiMarkdown or Pandoc, the resulting HTML is barebones. Unless I reference a style sheet explicitly in the front-matter block, it doesn't include any styling.

---
"`yaml
Title: Chris Gardiner-Bill Resume
Author: Chris Gardiner-Bill
Date: 2023-01-20
css: ../styles.css #This is my stylesheet
---
```

MultiMarkdown and Pandoc both accept a CSS file to style the resulting HTML. For my purposes, I required only some basic styling and layout to achieve the print-friendly design I wanted.

"`css
html {
    font-family: Optima;
    font-size: 11pt;
}

h1 {
    text-align: center;
    margin-bottom: 6pt;
    padding-bottom: 0;
}

h1 + p, h1 + p + p {
    text-align: center;
    margin-bottom: 0;
    padding-bottom: 0;
    margin-top: 0;
    padding-top: 0;
    font-size: larger;
}

h2 {
    border-bottom: 1px solid black;
    margin-bottom: 3pt;
    padding-bottom: 0;
}

h2 + p {
    margin-top: 0;
    padding-top: 0;
}

h3, h4 {
    margin-bottom: 0;
    padding-bottom: 0;
}

h4 + ul, h4 + p {
    margin-top: 0;
    padding-top: 0;
}

code {
    background-color: #e0e0e0;
    color: #212121;
    border-radius: 0.6rem;
    padding: .3rem .6rem;
    font-family: "SFMono-Regular","Consolas","Liberation Mono","Menlo",monospace
}

h2 + ul {
    column-count: 3;
}

hr {
    display: none;
}
```

If you are familiar with CSS, you'll see how simple I've gone. Here's a screenshot of the resulting HTML as styled by CSS.

![](Screenshot%202023-01-21%20at%2010.38.35%20am.png)

## HTML and PDF conversion

Markdown's original purpose was to make writing for the web more accessible, so converting a Markdown document to HTML is easy. I prefer doing so on the command line, but plenty of apps (web, desktop and mobile) will do it for you.

In my case, I use MultiMarkdown on my Mac, but Pandoc is a great alternative, and both are open source and available on macOS, Linux and Windows either from a binary or using a package manager.

To convert the file, I run the following command:

"`bash
$ multimarkdown /path/to/resume.md -o /path/to/output.html
```

I have an `output` directory in my repository, but you can compile your files wherever suits you.

HTML doesn't work as an exchange format, so I need to convert the file to PDF, which requires additional styling and an HTML-to-PDF converter. I use [PrinceXML](https://www.princexml.com/), but the free and open-source [wkhtmltopdf](https://wkhtmltopdf.org/) works just as well — and I'll likely switch to when my older version of Prince gets too long in the tooth.

Prince and wkhtmltopdf work similarly. Both convert HTML to PDF and use CSS's print media selectors to format page settings and create a print style and format distinct from the default or screen styles.

For my taste, I added very little to my CSS file, and while the following is Prince-specific, I think wkhtmltopdf has very similar requirements.

"`css
@media print {
    @page {
        font-family: Optima;
        size: a4;
        margin: 2cm;
        @bottom-center {
            content: counter(page);
        }
    }
    a:visited, :active, a:link {
        color: black;
        text-decoration: none;
    }
}
```

In the snippet above, the @page directive formats the page size (A4), margins and inserts a page number. Additionally, I style all hyperlinks black without the underlines.

Converting the HTML to PDF is then as simple as running the following command:

```bash
$ prince path/to/resume.html
```

The resulting PDF looks like this, as rendered in Preview.app on macOS — ready for me to share with a recruiter or potential employer.

![](Screenshot%202023-01-21%20at%2011.12.29%20am.png)

Pandoc can also create a Microsoft Word version (DOCX), which I don't need, but I might address this topic later.

### Alternative 

A clever alternative is to use a GitHub Action or Bitbucket Pipeline, depending on where you host your git repository, to build (and publish) your document. Action and Pipelines are automatically triggered by an event — for example, committing a new version of your resume to Git.

I'll return to this alternative in a later post.

## Branching for jobs

Git is the world's most popular version control software, favoured by developers for managing source code. Since Markdown is just a text file decorated with markup, it's ideally suited to working with Git.

There are a million ways to do this, but I've settled on using branches each time I customise my resume for a new job application.

To make this work, I created an extensive, rambling resume with every skill, capability, and experience I have attained throughout my career. I store this master resume within the repository's `master` branch.

Then, each time I want to apply for a new job, I create a branch and customise the resume specifically for the job requirements. For example, I recently applied to Atlassian, so I made an Atlassian branch in my repository as follows:

"`bash
$ git checkout -b atlassian
```

When I change my resume in this branch, it's independent of my master branch and any other branches I've created. Not only does it then allow me to build a customised resume, but it also preserves this version if I want to refer to it in future. I can even branch off an existing branch, for example, if I apply to a different job advertised by the same company in future.

Suppose I write something I like in a branched resume I want to reuse in my master branch. In that case, I check out the master branch (`$ git checkout master`) and make my changes as required, either manually or using `git cherry-pick` to pluck the changes of a specific commit into the branch.

## Concluding thoughts

This approach isn't for everyone, but it makes sense for a technical writer or software developer since most of us already use Markdown and git daily. It's also an excellent opportunity to practice what we preach and demonstrate that we can deliver what we say to clients and employers.

The only caveat is that many recruiters, even those placing candidates in technical positions, still do their job like it's 2005. Microsoft Word is their preferred medium of exchange — primarily, so they can edit and remove your contact details at their will. It is certainly possible to convert Markdown to DOCX, as I noted above, but I'll always submit my resume in PDF if it's allowed, and this workflow is my preferred way of doing so.

I may follow up this post with a DOCX and GitHub Actions/Bitbucket Pipelines workflow, so reach out if that interests you.