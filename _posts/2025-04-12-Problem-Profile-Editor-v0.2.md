---
title: Problem Profile Editor Update (v0.2)
description: Simple update to the package to select inaccessible profiles.
# date: 2025-03-15 14:00:00 -0500
categories: [Salesforce, LWC]
tags: [problem profile editor]     # TAG names should always be lowercase
pin: true
# image:
#   path: /assets/img/automation-add-on/teaser.png
#   alt: Example view of the Automation Add-On components.
---

Amazing and simple update to our **Problem Profile Editor**—haven't heard of it? Check out [the original post]({% link _posts/2025-03-13-Problem-Profile-Editor.md %}).

Received some great feedback from Michael over at [Free Like A Puppy](https://www.freelikeapuppy.tech/){:target="_blank"} <i class="fa-solid fa-arrow-up-right-from-square"></i> about a gap in the approach. When trying to deactivate a record type, Salesforce threw up the warning:
> This record type [Name of Record Type] cannot be deactivated because the following profiles use this record type as default.
{: .prompt-danger }

Unfortunately, one of these Profiles was the [Platform Integration User](https://help.salesforce.com/s/articleView?id=xcloud.security_platform_integration_user.htm&type=5)

> [Salesforce Help](https://help.salesforce.com/s/articleView?id=xcloud.security_platform_integration_user.htm&type=5) <i class="fa-brands fa-salesforce fa-lg" style="color: #74C0FC;"></i> tells us "_...you can’t view the detailed user record. Salesforce users don’t have permission to view or edit the Platform Integration User._"
{: .prompt-info }

This explained why the simple query of the Profile object wasn't returning this profile (and a few others).

A quick discovery idea came up with a way to find the record ids for this type of "inaccessible" profile:

```apex
SELECT Id, Name, ProfileId, Profile.Id, Profile.Name
FROM User
WHERE ProfileId != null AND Profile.Id = null 
```

Using this simple query, it wasn't hard to convert it to the GraphQL Syntax (my new favorite approach to avoid writing Apex for simple LWCs) and incorporate the results to the picklist of Profiles.

As a reminder, I've decided to make this project an 🔓📦 unlocked package and the repository is open-source.

<i class="fab fa-github"></i> [/tschug/ProblemProfiles/](https://github.com/tschug/ProblemProfiles)

But if you're curious, here's what the GQL version of the SOQL looks like as an LWC's GraphQL @wire adapter.

```javascript
@wire(graphql, { query: '$gqlQueryUserProfile', variables: "$params" })
    graphqlQueryResultUserProfiles(result) {
        this.gqlUserProfileData = result;
        let data = result.data;
        let errors = result.errors;
        if (data) {
            let result = data.uiapi.query.User;
            let records = result.edges.map((edge) => {
                let field = edge.node;
                return {
                    "value": field.ProfileId.value,
                    "label": field.Name.value,
                }
            });
            this.addOptions(records);
            this.isLoaded = true;
        } else if (errors) {
            console.log('GQL WIRE FAILED');
            console.log(JSON.stringify(errors));
        } else {
            this.profileOptions = [];
        }
    }

    get params() {
        return {};
    }

    get gqlQueryUserProfile() {

        return gql`
            query userprofiles {
                uiapi {
                    query {
                        User(
                            first: 100
                            where: {
                                ProfileId: { ne: null }
                                Profile: {
                                    Id: { eq: null }
                                }
                            }
                            orderBy: {
                                Name: { order: ASC }
                            }
                        ) {
                            edges {
                                node {
                                    Id
                                    Name { value }
                                    ProfileId { value }
                                }
                            }
                        }
                    }
                }
            }
        `;
    }
```

## Installation Info

v0.2 - 2025-APR-12
Sandbox & Scratch Orgs
: [https://test.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC](https://test.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC)

Production & Developer Edition Orgs
: [https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC)