---
title: Automation Add-On
description: Intrduction and overview to my Automation Add-On app for Flows.
# date: 2024-03-15 14:00:00 -0500
categories: [Salesforce, Flow]
tags: [flow, automation add-on]     # TAG names should always be lowercase
pin: true
image:
  path: /assets/img/automation-add-on/teaser.png
  alt: Example view of the Automation Add-On components.
---

📢 [Jump to Installation Info](#installation-info)

With the Summer '24 Release, Salesforce has introduced the [Automation Lightning App](https://help.salesforce.com/s/articleView?id=release-notes.rn_automate_flow_builder_automation_lightning_app.htm&release=250&type=5){:target="_blank"}

> View and monitor your flows in the Automation Lightning app, which is now available to all flow admins and other users that you grant access to. From the app, you can create flows or open any flow that you have permission to access in Flow Builder. New list views show your most recently modified flows and flow definitions that include errors. Search for flows using a keyword in the flow label. Filter or sort flows by type, progress status, last modified date, last modifying user, and associated record fields. Additionally, find a direct link to the Trailblazer Community and to relevant learning material on Trailhead.

Bringing Flows outside of Setup has been a personal project of mine ever since I learned you can do a SOQL query to get Flow related data:

- [FlowDefinitionView](https://developer.salesforce.com/docs/atlas.en-us.250.0.object_reference.meta/object_reference/sforce_api_objects_flowdefinitionview.htm){:target="_blank"}
- [FlowInterview](https://developer.salesforce.com/docs/atlas.en-us.250.0.object_reference.meta/object_reference/sforce_api_objects_flowinterview.htm){:target="_blank"}
- [FlowTestResult](https://developer.salesforce.com/docs/atlas.en-us.250.0.object_reference.meta/object_reference/sforce_api_objects_flowtestresult.htm){:target="_blank"}
- [FlowTestView](https://developer.salesforce.com/docs/atlas.en-us.250.0.object_reference.meta/object_reference/sforce_api_objects_flowtestview.htm){:target="_blank"}
- [FlowVariableView](https://developer.salesforce.com/docs/atlas.en-us.250.0.object_reference.meta/object_reference/sforce_api_objects_flowvariableview.htm){:target="_blank"}
- [FlowVersionView](https://developer.salesforce.com/docs/atlas.en-us.250.0.object_reference.meta/object_reference/sforce_api_objects_flowversionview.htm){:target="_blank"}

There are even more, plus there are a few new ones that are now accessible to most orgs as of Summer '24.

## Introducing Automation Add-On

Since Salesforce beat me to the punch by exposing Flows outside of Setup, I have pivoted and released an suite of Lightning Web Components (LWCs) that will further enhance the Automation Lightning App.

1. Advanced Details
1. Schedule-Triggered Details
1. Triggered Details (Record-Triggered and Platform Event-Triggered)
1. Time-Based Monitoring (Scheduled Paths)
1. Test Lists
1. Variable List
1. Paused Interview List
1. Categorization

## Components

### Flow Advanced Details

Flow Advanced Details displays about the state and conditions of the Flow.

![Advanced Details](/assets/img/automation-add-on/advanced-details.png)

#### Badge Types

Based on the state of the Flow, these are the badges that can appear:

![Badge Types](/assets/img/automation-add-on/badge-types.png)

_Absolute Values_:

- Status Badge
  - **Active**:  The Flow has an activated version
  - **Inactive**:  The Flow has no active version
- Version Badges
  - Version Number
    - Displays what is the most recent or currently active version number
  - Current Version Status
    - **Using Current Version**:  The latest version is the one which would be used
      > If there is no active version of the Flow, this will always appear because it assumes the latest version is the one being developed or will be used
      {: .prompt-info }
    - **Using an Older Version**:  When the active version is not the same as the latest version, this will appear to alert the user.

_Conditional Values_:

- Template-related Badges
  - **Is Template**:  The Flow version is designated as a template
  - **From Template**:  The Flow version is assigned to a template, likely from using that template to build the Flow
- Override-related Badges
  - **Is Overridable**:  The Flow version is designated as overridable
  - **Overridden**:  The Flow is actively being overridden by another Flow
  - **Overrides Another Flow**:  The Flow version indicates that it is being used to override another Flow which is overridable
- Async Badge
  - **Async Path**:  The Flow has an asynchronous path.
    > [Winter 22 Release Notes:  Asynchronous Path](https://help.salesforce.com/s/articleView?id=release-notes.rn_automate_flow_builder_asynchronous_path.htm&release=234&type=5){:target="_blank"}
    {: .prompt-tip }
- Package State Badge
  - **Beta**: Indicates that for a package org, the Flow is currently in a Beta release.
  - **Deleted**: ??
  - **Deprecated**: ??
  - **Deprecated** Editable: ??
  - **Installed**: Indicates the Flow is installed via a package.
  - **Installed** Editable: Indicates the Flow is installed via a package and is editable.
  - **Released**: Indicates that for a package org, the Flow is currently in a release.
  - **Unmanaged**: Indicates that the Flow is not in nor from a package.
- Package Namespace Badge
  - Provides the namespace prefix of the Flow if installed by a package with a namespace.
  - This may seem redundant with the Installed package state badge, but some packages do not have namespaces.

Each set of badges above can be shown or suppressed based on configuration via the property editor.

![Advanced Details Property Configuration](/assets/img/automation-add-on/advanced-details-properties.png)

#### Open Source Flow Button

Flows from a template, being overridden, or overriding another Flow, the name of the source Flow and a button to open it in a new browser tab can be shown or suppressed based on configuration via the same property editor.

![Opening a Source Flow](/assets/img/automation-add-on/open-source-flow.png)

### Flow Schedule Details

Flow Schedule Details displays details about the schedule and settings.  This LWC will only render if the Flow is a Schedule-Triggered Flow.
“Launches at a specified time and frequency for each record in a batch. This autolaunched flow runs in the background.”

![Schedule-Triggered](/assets/img/automation-add-on/schedule-details.png)

> Help text tips have been added to indicate to the user that the Schedule is in the org’s timezone, and that timezone is displayed when hovering over the tool tip.
>
> The Run Times are in the user’s timezone, but hovering over the tooltip will display the org’s time.
{: .prompt-tip }

### Flow Trigger Details

Flow Trigger Details displays information about the triggering event (Platform- or Record- Triggered). This LWC will only render if the Flow is a Schedule-Triggered Flow.

For Platform Event-Triggered Flows, it appears like this:

![Platform Event-Triggered](/assets/img/automation-add-on/platform-event.png)

For Record-Triggered Flows, it appears like this:

![Record-Triggered](/assets/img/automation-add-on/record-triggered.png)

For Record-Triggered Flow, it is also possible to display a button to open a pop-up modal for monitoring time-based Flow interviews.  This button can be shown or suppressed based on configuration via the property editor.

![Trigger Details Property Configuration](/assets/img/automation-add-on/trigger-details-properties.png)

The button will appear as follows:

![Record-Triggered Time-Based Monitoring](/assets/img/automation-add-on/record-triggered-monitoring.png)

When clicked, it will launch the Time-Based Workflow page from Setup.  There is no way to query these records, so in the spirit of this App, the Setup page is brought to the user and pre-populated with the search fields related to the Flow record.

![Record-Triggered Time-Based Monitoring Modal](/assets/img/automation-add-on/time-based-monitoring-modal.png)

### Flow Time-Based Monitoring

Flow Time-Based Monitoring displays the Time-Based Workflow page from Setup.  There is no way to query these records, so in the spirit of this App, the Setup page is brought to the user and pre-populated with the search fields related to the Flow record.

> This component is used in the pop-up modal launched by the Record-Triggered button for time-based monitoring, so there is no need to use both.
>
> You'll notice in the image below, we added a new tab on the page layout for this component.  With Summer '24, you can also make these tabs conditionally displayed and you may want to explore that.
{: .prompt-tip }

![Time-Based Monitoring](/assets/img/automation-add-on/time-based-monitoring.png)

> This component is only utilized for record-triggered Flow and will display a message if the Flow is not relevant to this component.
{: .prompt-info }

![Time-Based Monitoring Unavailable](/assets/img/automation-add-on/time-based-monitoring-unavailable.png)

### Flow Variables List

Flow Variables List displays a list of all input/output variables.  Help text will remind users that it’s only these types of variables that are displayed.

> **Reminder**:  Only input and output variables will be shown.
{: .prompt-info }

![Variables List](/assets/img/automation-add-on/variables.png)

### Flow Tests List

Flow Tests List displays the status of Flow Tests for the current version. This LWC will only render if the Flow has Flow Tests, which is currently limited to certain record-triggered flows.

> [Source: Salesforce Help Docs:  Testing Your Flow](https://help.salesforce.com/s/articleView?id=sf.flow_concepts_testing.htm&type=5){:target="_blank"}
{: .prompt-info }

![Test List 1](/assets/img/automation-add-on/tests.png)
![Test List 2](/assets/img/automation-add-on/tests2.png)

### Flow (Paused) Interviews List

Flow Interviews List displays any existing Flow interviews in a Paused state.  This tries to closely mimic what is seen in Setup for the Paused and Failed Flow Interviews.

Users will be able to use a row action to select from two options which will open a new tab.

![Paused Interviews List](/assets/img/automation-add-on/interviews.png)

- **Inspect** - will open a tab that shows the current position of the paused Flow on the Flow Builder Canvas, along with its Debug Details.
  > This is helpful to see exactly where the paused Flow is at in a process.
  {: .prompt-tip }

![Inspection](/assets/img/automation-add-on/inspection.png)

- **Show Details** - will open a tab that navigates directly to the Flow Interview page in setup, as if the user navigated to it directly from the Paused and Failed Interviews page.  This allows the user to either delete the paused Flow or resume it.
  > It was attempted to embed this page inside of a modal like the Time-Based Monitoring; however, the Setup page does not behave correctly when outside of Setup, regardless of if it is in a modal, iframe, or by itself.  Until this is resolved, direct navigation to Setup is the best option.
  {: .prompt-info }

![Show Details](/assets/img/automation-add-on/show-details.png)

### Flow Categorization

Flow Categorization is a special component to enable users to easily access and manage (even control) the Category and Subcategory text fields on the Flow record.

This component can be a challenge to understand, but once it’s been used once or twice, it’s fairly easy to comprehend (and greatly appreciated).

Through the relatively easy configuration of a special delimited file (such as a CSV or comma-separated-variable file) stored as a Static Resource, the users will be shown a custom set of dependent picklists.  The category options are actually a set of labels/values and optional descriptions.  

> [See how the special delimiters are configured and how to access the packaged example](#delimited-file-structure).
{: .prompt-tip }

The component can be configured with a few options:

![Categorization Property Configuration](/assets/img/automation-add-on/categorization-properties.png){: .right }

- Show Card Header

This allows the LWC’s card header to appear or not.  This is because the component was designed intentionally so that it could fit in the Lighting Page Layout to appear as if it’s part of the page.  More detail to replace the entire page was not built until feedback is received and more is learned about the Flow record object.

- Always Allow Manual Entry

The component has been intentionally designed to support the underlying factor that the data is saved in text fields and not picklists.  As such, if the existing value in either field does not match the configured values for the picklists, the input is rendered as a manual text entry to ensure this “backwards” compatibility.  This manual text entry can be always enabled or only used when necessary as designated above.  Ideally, the desired values are configured and only picklists are shown to the users.

- Category File Name

This is the API name of the Static Resource so that the component can retrieve the contents of the special delimited file.

- Category File Namespace

It’s desired to provide a recommended set of Flow Categories based on feedback.  In order to access any static resource in a namespace package and avoid duplicate issues, an optional namespace parameter has been added.

- Category File Delimiter

As mentioned, this is a special type of delimited file and the delimiter indicates how the individual options are separated.

- Category Value Splitter

This is the character/delimiter that separates the label/value from the optional description.

![Categorization-Picklist](/assets/img/automation-add-on/categorization-picklist.png)

![Categorization-Picklist](/assets/img/automation-add-on/categorization-subcategory.png)

When the input mode supports both picklist and manual entry, a toggle is shown to the right of the input.

- The icon of a picklist with a cloud designates that a picklist is being presented.
- The icon of a text input field in blue designates that the input is in manual entry mode.

![Categorization Manual](/assets/img/automation-add-on/subcategory-manual.png)

In the image below, always allowing manual entry has been disabled, but because the existing Subcategory does not meet the configured delimited input options, it is displayed in manual entry mode.  Toggling the button to the right will display the allowed subcategories for the category.

![Categorization Manual Loaded](/assets/img/automation-add-on/subcategory-manual-loaded.png)

When saving the categorization to the record, the component will attempt to save the data back to the record and the user will be informed if it was successful or failed.

![Categorization Saved](/assets/img/automation-add-on/categorization-saved.png)

> When using this component, we recommend marking the fields on the Page Layout (different from Lightning Record Page) as read-only.  The System Administrator will still have edit acces, but others should not.
>
> To access the Page Layout, you cannot do this from Setup -> Object Manager like normal Salesforce objects.  Instead, you must follow these steps:
> 1. Navigate to the `Automation` app from the App Launcher
> 1. Select the `Flows` tab
> 1. Click the `Setup` gear in the upper right corner, ans select `Edit Object`
> 1. You now have access to the `FlowRecord` in Object Manager
> 1. Select `Page Layouts`
> 1. Click on `Flow Layout`
> 1. Find the Category field, then when your cursor is hovering over the field, select the wrench icon 🔧 to edit the properties
> 1. Check the box for `Read-Only` and click OK
> 1. Repeat this process for the Subcategory field
> 1. Click the Save button
{: .prompt-tip }
> **Known Glitch**: If you remove the fields from the Page Layout, there is often a "refresh" issue that makes the component not reflect the changed values.  That's why we offer the recommendation above, rather than removing them from the Page Layout.
>
> We are also aware that there is behavior where Subcategory can be manually entered without a Category and this can create a confusing user interface loop when trying to clear the value if setting it to Picklist mode.
{: .prompt-danger }

## Setup

### Build a Lightning Record Page

Using the Lighting App Builder is another option for managing pages.  See the [Salesforce Help Docs: Lightning App Builder for more information](https://help.salesforce.com/s/articleView?id=sf.lightning_app_builder_overview.htm&type=5){:target="_blank"}.

1. Click the Setup gear in the upper righthand corner and select Setup

   ![Setup Gear](/assets/img/automation-add-on/setup-gear.png)

2. Search for `Lightning App Builder` in the Quick Find.

   ![Setup Quick Find](/assets/img/automation-add-on/setup-quick-find.png)

3. Click `New` to build an entirely new page

   ![Lightning App Builder](/assets/img/automation-add-on/setup-lightning-app-builder.png)

4. Select `Record Page` and click `Next`.

   ![New Lightning Page](/assets/img/automation-add-on/create-new-lightning-page.png)

5. Provide a Label and select the object as `Flow (FlowRecord)`, then click `Next`.

   ![Select FlowRecord](/assets/img/automation-add-on/object-flowrecord.png)

6. Select the desired `Page Template` or `Clone a Salesforce` and then click Done.

   > **Recommendation:**  Clone Salesforce Default Page -> Grouped View Default
   {: .prompt-tip }

   ![Clone Salesforce Default Page](/assets/img/automation-add-on/clone-default-page.png)

7. Located the `Custom - Managed` components on the left hand menu

   ![Custom Managed Components](/assets/img/automation-add-on/custom-managed.png)

8. Drag the components onto the page.

   - First add the `Flow Details Publisher`.

      > This MUST be located on the “outer” area of the page layout canvas.  It must not be located on any tabs on the page.
      >
      > This LWC will send the other LWCs the data needed for them to render correctly.
      {: .prompt-danger }

   - Then drag the other desired LWCs wherever you want them to appear, our recommendations are below:

   ![Lightning Page Layout](/assets/img/automation-add-on/page-layout.png)

      1. Right column
         - Flow Classification ***(See tip below)***
         - Flow Advanced Details
         - Flow Schedule Details
         - Flow Trigger Details
         - Flow Details Publisher ***(Required)***
      2. Related tab, under the `Flow Versions` related list.
         - Flow Tests List
         - Flow Variables List
         - Flow Interviews List

  > For the **Flow Categorization** LWC, we recommend first reviewing the component by using the included demonstration file with sample categories and subcategories.  You can use it by setting the following values:
  > - **Category File Name**:  `demo_flow_categories`
  > - **Category File Namespace**:  `autom8n`
  > - **Category File Delimiter**:  `,` (it should default to this comma character)
  > - **Category Value Splitter**:  `:` (it should default to this colon character)
  > See the [screenshot above in the Components: Flow Categorization](#flow-categorization) section if you need help understanding the configuration.
  >
  > 📢 **Also** when using this component, we recommend making the fields on the Page Layout (different from Lightning Record Page) as read-only.
  {: .prompt-tip }

9. Save the page.

10. Activate as needed. See the [Salesforce Help Docs: Activate Lightning Experience Record Pages](https://help.salesforce.com/s/articleView?id=sf.lightning_app_builder_customize_lex_pages_activate.htm&type=5){:target="_blank"} for more details.

### Configure Categorization Picklist Options

Once you understand how the Flow Categorization LWC works, you can upload your own file as a Static Resource and configure the component as described above by setting the appropriate properties.

You can download our sample categorization file if you'd like to review.  See [Delimited File Structure](#delimited-file-structure) for find it and for details on how to build your own file.

## Delimited File Structure

The delimited file is special in that it actually has two delimiters.  The structure of the file is as follows:

1. The first column, or the first “value” in a row, is for the Category options, while all other columns are for the Subcategory options that depend on that Category.
1. These are separated by the `Delimiter`
1. Options mean:
   - the label displayed and value assigned to the Flow’s category or subcategory field
   - (optional) a description to appear in the picklist under the label/value to give users more clarity on why or why not it should be used
1. Options are separated by the `Splitter` to “split” the label/value from its description

> Example:
>
> - Delimiter is a comma
> - Splitter is a colon
{: .prompt-info }

```txt
Category 1: Description of the first category, Subcategory 1A: Description of 1A, Subcategory 1B: Description of 1B
Category 2 without description, Subcategory 2A without description, Subcategory 2B with no description
Category 3: With a description, Subcategory 3A without description, Subcategory 3B: This has a description
Category 4 without description, Subcategory 4A: This has a description, Subcategory 4B without description
```

It should be clear that it’s best to be as consistent as possible to not confused users, whether that means:

- All-or-no descriptions
- Only-category or only-subcategory descriptions
- Certain subcategories of a category have all-or-no descriptions

You can access the packaged sample by navigating in Setup to the Static Resources

![Static Resource](/assets/img/automation-add-on/static-resource.png)

![Sample Categorization File](/assets/img/automation-add-on/demo-csv.png)

## Installation Info

Sandbox & Scratch Orgs
: [https://test.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001S5QDAA0](https://test.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001S5QDAA0)

Production & Developer Edition Orgs
: [https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001S5QDAA0](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001S5QDAA0)

## Uninstall Steps

If you followed the steps we've outlined here, it should be simple:

1. Navigate to Setup
1. In the Quick Find, search for `Lightning App Builder` and select it
1. Find the Lightning Record Page you built
1. Click on `Edit` next to it
1. Deactiveate the page
   1. Click on `Activation`
   1. Click the appropriate `Remove as...` (should be Org Default)
   1. Follow the steps on the screen
1. Click the Lightning App Builder's back arrow (top left)
1. Find the Lightning Record Page (again)
1. Click on `Del` to delete it
1. Now uninstall the package
   1. In the Quick Find, search for `Installed Packages`
   1. Click `Uninstall` next to the Flow Automation Add-On package (autom8n)
   1. Follow the steps