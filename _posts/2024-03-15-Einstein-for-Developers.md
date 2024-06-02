---
title: First Take&#58;  Einstein for Developers
# date: 2024-03-15 14:00:00 -0500
categories: [Salesforce, Developer]
tags: [developer, einstien, vscode]     # TAG names should always be lowercase
---

As you'll find out about me, I am cautiously optimistic about Salesforce, especially about beta and new functionality. I find excitement and joy in seeing where Salesforce is headed, what predictions I can make from them, and more importantly, how I can leverage them&mdash;especially for nonprofits.

A few days ago my work bestie started to explore [Einstein for Developers (at the time of writing, it's in Beta)](https://developer.salesforce.com/tools/vscode/en/einstein/einstein-overview) using VS Code, and I thought I should share in the exploration.

The overview linked above boasts a few things and I've tried them out.

> Einstein for Developers is an AI-powered developer tool that’s available as an _**easy-to-install Visual Studio Code extension**_ built using CodeGen, the secure, custom AI model from Salesforce. The extension is available in the VS Code marketplace and the Open VSX registry. Note that the extension _**does not use customer data to train**_ our LLM.
>
> Einstein for Developers _**assists you throughout the Salesforce development process with expertise learned from anonymized code patterns**_. Our suite of AI-powered developer tools _**increases productivity and provides helpful assistance for complex coding tasks**_. We _**enforce development best practices**_ with code generation and our suite of recommended static analysis and security scanning tools. With boilerplate code generation as its foundation, AI-assisted tooling also _**makes it easier for new developers to onboard to the Salesforce Platform**_.

## Current Capabilities

1. Generate Apex Code from Natural Language Prompts
: Enter natural language instructions in a sidebar, so you can work with your editor and the tool side by side, without any interruptions to your workflow. Or use the VS Code Command Palette to enter a prompt describing what you’d like to build and then generate Apex code suggestions within your editor.

2. Inline Autocomplete for Apex and LWC
: Get multiple inline suggestions as you code. Easily pick the suggestion that works for you. This feature is available in Apex and LWC (Javascript and CSS) files.

3. Test Case Generation for Apex
: Use Einstein for Developers to quickly generate unit tests for your Apex methods.

## My Opinions

### Generate Apex Code from Natural Language Prompts

**3 out of 10**.

You must be very articulate in what you're asking in your prompt.  When I tried it out, I found that requests for complex code generation were not great.  When asked for code snippets, it was a little better, but still hard to pin point with the proper context.

It was hit or miss, but overall helpful (which is why it's not a 2) with addressing my lack of JavaScript knowledge when working on an LWC.

### Inline Autocomplete for Apex and LWC

**7 out of 10**.

I have a very unique style of typing when I code and often pause for both my fingers and thoughts to catch up.  As a result, Einstein trys to suggest things during this pause.  I found it annoying most of the time.  It was like having a junior developer sitting in a chair over my shoulder and blurting out what they think I'm trying to do.  This is more often then not where I was going with my code.

Sometimes, I was pleasantly surprised, as it pointed me to approaches I had not considered and at other times it knew exactly what I was trying to do and saved me some time.  It was even helpful the majority of the time trying to write comments for me.

The higher rating given is the result of knowing myself and when I should enable and disable this feature with the conveniently located icon or command palette.

### Test Case Generation for Apex

**5 out of 10**.

For simple tests, this was fine.  However, I have adopted a build class approach for test classes and Einstein didn't know anything about how to work it.  It did attempt to, but struggled to know how to use it.  

I did like the fact that I could at least start my test classes from the class and this did help save some time of having to type out the wrote memorization of the syntax.

Again, it won't be for everyone or every situation, but I can see its potential.

### Overall

Personally, I won't rule it out for the future use, and it won't be for everyone.  But it's definitely promising.
