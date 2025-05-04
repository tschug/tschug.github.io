---
title: Conditional Formatting to Record Fields on Dynamic Forms
description: The .
# date: 2025-03-15 14:00:00 -0500
categories: [Salesforce]
tags: []     # TAG names should always be lowercase
pin: false
# image:
#   path: /assets/img/automation-add-on/teaser.png
#   alt: Example view of the Automation Add-On components.
---

🚨Spoiler alert🚨 - TL;DR (Too long; didn't read)

Modify the `UiFormatSpecificationSet` metadata and deploy it using the command line interface (CLI) in order to:

- Reorder rules inside the ruleset
- Evaluate if a field is blank in rules
- Use cross-object fields in rules

📢 [Jump to Workarounds & Hacks](#workarounds--hacks)

# Conditional Field Formatting

The [Winter '25](https://help.salesforce.com/s/articleView?id=release-notes.rn_lab_conditional_field_formatting.htm&release=252&type=5) release brought us an opportunity to "make record fields stand out" with Conditional Formatting.

These excited and terrified me having worked through the Open-Source Commons program on the [Salesforce Indicators project](https://sfdo-community-sprints.github.io/indicators-documentation/){:target="_blank"} <i class="fa-solid fa-arrow-up-right-from-square"></i> which had a similar goal of drawing the users' attention about data on the record.

>[Salesforce Help](https://help.salesforce.com/s/articleView?id=platform.conditional_formatting_overview.htm&type=5) <i class="fa-brands fa-salesforce fa-lg" style="color: #74C0FC;"></i> tells us we can "make a field stand out by adding special formatting based on rules and criteria you provide, so that end users can quickly identify the most relevant information on a record page. You can apply conditional formatting to almost any field on a Dynamic Forms-enabled page, including the fields in the Dynamic Highlights Panel component.
{: .prompt-info }


## Purpose

I was recently trying to create staging objects for data that would be sent to another system using HTTP Callouts and Flow.  In order to make the experience easier for users, I wanted to bring attention to fields on the record and Conditional Field Formatting seemed like the right call, since I was trying to include this in a package.

I learned a lot and wanted to share.

Conditional Field Formatting is young but pleanty has been written about it, so I won't focus on the common concepts of what it is or how to set it up.

**Instead, let's take a dive into some setbacks, the technical side, and some creative workarounds, both inside the tool and outside.**

> Check out these well-known blogs to learn the ins and outs of setting up Conditional Field Formatting.
>
> - [Automation Champion: <br/>Conditional Field Formatting to Visually Highlight Fields for Quick Attention](https://automationchampion.com/2025/03/10/conditional-field-formatting-to-visually-highlight-fields-for-quick-attention-2/){:target="_blank"}&nbsp;<i class="fa-solid fa-arrow-up-right-from-square"></i>
> - [Salesforce Ben: <br/>Your Guide to Conditional Formatting for Salesforce Fields](https://www.salesforceben.com/your-guide-to-conditional-formatting-for-salesforce-fields/){:target="_blank"}&nbsp;<i class="fa-solid fa-arrow-up-right-from-square"></i>
{: .prompt-tip }

## Backstory

The situations I needed to address were:

**First, was the external tool's unique id field populated?** Users would select an Account record from a lookup, but these external ids for the tool are stored in custom metadata. So they get added to the record using a before-save flow.  We couldn't make the field required because that would prevent saving if they didn't know the value.  We also didn't want the flow to fail if it couldn't find a value, so that's why the field could be empty.

I learned that you can't have a rule to check if a field has a value or is blank.

**Second, was the parent record okay?** When viewing a child record, we wanted to give an indication to the user if the parent record's status was set to error, ready to be exported, or successfully exported.

I learned that you can't use parent record data in a rule, nor can you apply a conditional format to the related field when used on a page.

## Gaps and Setbacks

There are a lot of known gaps/setbacks of Conditional Field Formatting.  Salesforce tells us that only these field types are supported:

- String type fields: Autonumber, Currency, Email, Number, Percent, Phone, Text, Text Area, URL
- ID
- Checkbox (boolean)
- Geolocation
- Picklist
- Formula fields that resolve to one of these preceding types
- Roll-up summary fields that resolve to one of these preceding types

🚨Spoiler alert🚨 - You **CAN** use Lookup/Master-Detail relationship fields in your rule, just not from the Salesforce UI!

And only supports the operators:

- `CONTAINS`
- `=` and `==` (Equal)
- `<>` or `!=` (Not Equal)
- `>` (Greater Than)
- `>=` (Greater Than or Equal)
- `<` (Less Than)
- `<=` (Less Than or Equal)

Also, they tell us that because it relies on Dynamic Forms, conditional field formatting can appear on mobile only when Dynamic Forms is enabled for mobile.

Rules are executed in the order listed. When a rule evaluates to True, the remaining rules are ignored.

### Stated Considerations

Salesforce calls out things to consider with Conditional Field Formatting:

- You can apply conditional formatting only to fields on objects that support Dynamic Forms.
- A limited set of icons are available for conditional formatting and come from the Lightning Design System.
  - Happy Face
  - Smiling Face
  - Neutral Face
  - Sad Face
  - Loading Spinner
  - Warning Triangle
  - Rocket
  - Chat Bubble
  - Chat Bubble with SMS
  - Solid Circle with Check
  - Solid Circle with X
  - Question Mark
  - Home
  - Solid Star
  - Hollow Star
  - Megaphone
  - Heart
  - Thumbs Up
  - Thumbs Down
  - Buildings
  - Product
  - Diamond
  - Square
  - Circle
- If you apply conditional formatting to a field, but it doesn’t appear when the page displays at run time, then conditional formatting isn’t supported for that field.
- Conditional formatting rules can evaluate differently at run time, depending on the permissions of the user viewing the record page.
  - Meaning, that if users do not have access to the fields used in filter criteria, then that rule is ignored and Salesforce continues on to the next rule for evaluation.
  - Therefore, it’s possible that one user sees a different result for conditional formatting than their colleagues.
- Conditional formatting rules evaluate based on the raw value of a number, not its rounded up display value.
- Conditional field formatting rules treat blank (null) values on numerical fields as zero values.
- Conditional formatting rulesets aren't automatically included when you add a Lightning page to a change set.

### Stated Limitations

Salesforce also calls out limitations:

- A Lightning page can have up to 15 unique rulesets.
- A ruleset can have up to 10 rules.
- A rule can have up to 10 conditions. If you don’t specify any conditions, the rule always applies.
- You can’t add conditional formatting to [cross-object fields](https://help.salesforce.com/s/articleView?id=platform.dynamic_forms_spanning_fields.htm&type=5), nor can you select cross-object fields when creating rules.
  - Displaying related object field values is an amazing [Spring '24 release feature](https://help.salesforce.com/s/articleView?id=release-notes.rn_lab_dynamic_forms_crossobject_fields.htm&release=248&type=5)
  - 🚨Spoiler alert🚨 - You **CAN** use cross-object fields in your rules, just not from the Salesforce UI!

### Other Discovered/Observed Issues

- Limited color options, not just limited icons
  - Gray, Blue, Green, Orange, Red, Purple
- Cannot reorder the rules once entered
  - 🚨Spoiler alert🚨 - You **CAN** reorder the rules, just not from the Salesforce UI!
- No insight on what the icon/color means.
- Can only create rule sets from the Lighting App Builder.
  - Thankfully we can edit them in Setup by navigating to the Object, then selecting Conditional Field Formatting from the leftside navigation menu.
  - This is also the only way to delete them.
- Conditional field formatting rules don't let you save a blank (null) value because it's a required input field.
  - 🚨Spoiler alert🚨 - You **CAN** evaluate if a field is blank in your rule, just not from the Salesforce UI!
- Trouble with packaging
  - Personally, I encountered struggles trying to include a layout that uses conditional field formatting in a namespaced package.

## Metadata

With this functionality, we got a new metadata type **UiFormatSpecificationSet**, otherwise referred to as a conditional formatting ruleset in the Salesforce documentation and UI.

>[Metadata API Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.252.0.api_meta.meta/api_meta/meta_uiformatspecificationset.htm) <i class="fa-brands fa-salesforce fa-lg" style="color: #74C0FC;"></i> lets us know that the `UiFormatSpecificationSet` "represents a set of rules that define the style and visibility of conditional field formatting on Dynamic Forms-enabled Lightning page field instances."
{: .prompt-info }

Here's a sample of what that looks like, because it's important to know what we can/cannot do with it.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<UiFormatSpecificationSet xmlns="http://soap.sforce.com/2006/04/metadata">
    <field>Dynamic_Form_Demo__c.Checkbox__c</field>
    <formatType>ICON</formatType>
    <masterLabel>Checkbox Demo</masterLabel>
    <sobjectType>Dynamic_Form_Demo__c</sobjectType>
    <uiFormatSpecifications>
        <formatProperties>{&quot;icon&quot;:&quot;square&quot;,&quot;iconColor&quot;:&quot;red&quot;}</formatProperties>
        <formatType>ICON</formatType>
        <order>1</order>
        <visibilityRule>
            <criteria>
                <leftValue>{!Record.Checkbox__c}</leftValue>
                <operator>EQUAL</operator>
                <rightValue>false</rightValue>
            </criteria>
        </visibilityRule>
    </uiFormatSpecifications>
    <uiFormatSpecifications>
        <formatProperties>{&quot;icon&quot;:&quot;circle&quot;,&quot;iconColor&quot;:&quot;gray&quot;}</formatProperties>
        <formatType>ICON</formatType>
        <order>2</order>
        <visibilityRule>
            <booleanFilter>1 AND (2 OR 3)</booleanFilter>
            <criteria>
                <leftValue>{!Record.Checkbox__c}</leftValue>
                <operator>EQUAL</operator>
                <rightValue>true</rightValue>
            </criteria>
            <criteria>
                <leftValue>{!Record.Picklist__c}</leftValue>
                <operator>NE</operator>
                <rightValue>Yes</rightValue>
            </criteria>
            <criteria>
                <leftValue>{!Record.URL__c}</leftValue>
                <operator>CONTAINS</operator>
                <rightValue>Salesforce</rightValue>
            </criteria>
        </visibilityRule>
    </uiFormatSpecifications>
</UiFormatSpecificationSet>
```
{: file="force-app\main\default\uiFormatSpecificationSets\ChecklistDemo.uiFormatSpecificationSet-meta.xml" }

- `field` : Required. The object field that the conditional formatting is associated with.
- `formatType` : Required. The type of conditional formatting associated with the field. Currently the only value is a string enumeration `ICON`.
- `masterLabel` : Required. The label for the conditional formatting ruleset, which displays in Setup.
- `sobjectType` : Required. The object the ruleset is associated with.
- `uiFormatSpecifications` : The list of rules contained in the ruleset.

> So as you can see in this example, we're building a `Checkbox Demo` rulset on the `Dynamic_Form_Demo__c` custom object in order to apply an **Icon** to the `Checkbox__c` field.
{: .prompt-info }

Each `UiFormatSpecification` represents a single rule in the ruleset and has its own parts:

- `formatProperties` : Required. The properties for a given formatType in JSON format.
- `formatType` : Required. The type of conditional formatting associated with the field. Currently the only value is a string enumeration `ICON`.
- `order` : Required. A numerical value representing the conditional formatting rule’s position in the evaluation order.
- `visibilityRule` : A set of one or more filters that define the conditions under which the conditional formatting appears on the field.
If the visibility rule evaluates to `true`, the formatting displays on the field. If `false`, it doesn’t display. If this field is null, the formatting displays by default.

> For our demo, you can see that we have two specifications.
>
> Salesforce is very good at displaying the rule information to you, in this case we can see:
>
> - Rule 1 is for a red square icon
> - Rule 2 is for a Icon gray circle icon
{: .prompt-info }

Each `visibilityRule` or `UiFormulaRule` represents a set of one or more filters that define the conditions under which conditional field formatting displays on a Dynamic Forms-enabled Lightning page field instance.

- `booleanFilter` : Specifies advanced filter conditions.
- `criteria` : List of one or more filters that, when evaluated, determine conditional field formatting visibility.

> For our demo, you can see that Rule 1 has no advanced filter conditions, while Rule 2 has `1 AND (2 OR 3)`
{: .prompt-info }

For the `criteria` there are one or more `UiFormulaCriterion` which are a single filter that when evaluated, helps define conditional formatting visibility on a Dynamic Forms-enabled Lightning page field instance.

- `leftValue` : Required. The field upon which the filter is based.
- `operator` : Required. Defines the operator used to filter the data.
- `rightValue` : The value by which you want to evaluate the formatting visibility.

> For Rule 1 of our demo, the only criterion is that the `Checkbox__c` field equals `False`.
>
> For Rule 2 however, `Checkbox__c` field should equal `True` and either the `Picklist__c` field does not equal the value _Yes_ or the `URL__c` contains the value _Salesforce_.
{: .prompt-info }

## Workarounds & Hacks

Now that we understand how the metadata file looks, we can retrieve it from the Metadata API and begin to manipulate it.

### Reorder Rules Inside the Ruleset

As mentioned earlier, rules are executed in the order listed and the first one to evaluate to True is used. As we observed, you cannot simply reorder your rules, you would have to delete them and re-add them.  

This can be problematic if you want to later add a new rule in the middle. Something I experienced a lot during the process.

However, I discovered that we can go into the file and simply change the values within the `order` tags, all without having to move them around the file.

```xml
<order>1</order>
```

### Evaluate If a Field is Blank in Rules

If you're like me, you may have optional fields and you want to draw attention to users that they are blank.

In order to raise awareness to my users, I simply added a normal rule which was looking for a field to have a value. Then updated the metadata to not have a value for the right-value:

```xml
<criteria>
	<leftValue>{!Record.Account_External_Id__c}</leftValue>
	<operator>EQUAL</operator>
	<rightValue></rightValue>
</criteria>
```

or even

```xml
<criteria>
	<leftValue>{!Record.Account_External_Id__c}</leftValue>
	<operator>NE</operator>
</criteria>
```

### Use Cross-Object Fields in Rules

The most important thing for me was being able to display values related to the parent record. Even though it's not possible from the Salesforce UI, we can manipulate the `leftValue` value with the normal syntax we do in SOQL and other areas of Salesforce.

Here, I'm checking if the grand-parent account (hence two `.Parent` values) of the record's account field has a `Gold` service level agreement.

```xml
<criteria>
	<leftValue>{!Record.Account__r.Parent.Parent.SLA__c}</leftValue>
	<operator>EQUAL</operator>
	<rightValue>Gold</rightValue>
</criteria>
```

Now, in my use case I still added the cross-object fields to the page, so that the value is displayed. This is how I learned you cannot put Conditional Field Formatting on these types of fields, so I had to assign it to another field.

> 🎁 **Bonus Tip**
>
> To make this more user friendly and to hide these fields from the page when creating a new record, I set the Conditional Visibility to only show the field if the record's created by user's username was not blank.
> This is because when creating a new record, the `CreatedBy` field does not have a value.
{: .prompt-tip }

## Closing Thoughts

While it was good to learn about Conditional Field Formatting, I believe there is a time and place for its use.

It shines with its ability to use the field's value or on the values of other fields on the page to evaluate the formating. As well as a very smooth setup process, despite different actions requiring different areas of Salesforce.

However, in its current form, it is limited in its inability to check for cross-object or blank/null values.  Similarly, not being able to apply the conditional formatting to cross-objects on the page falls short.  In this situation, I had to add them to anothe field on the page.

I also wish that it provided context of why the icon appeared as such. Per the help docs:

> Conditional formatting is designed to amplify the information on the field that is already there. For best results and to ensure that the configuration is accessible, make sure the icon, icon color, and field value convey the same information, such as a green happy face on a field with a value of Positive.
{: .prompt-info }

For the above situations, I would encourage you to adopt or continue to use [Salesforce Indicators](https://sfdo-community-sprints.github.io/indicators-documentation/){:target="_blank"}. <i class="fa-solid fa-arrow-up-right-from-square">

Today, it supports:

- Every SLDS icons, execpt Action icons, and custom images
- Any HTML color
- Descriptive text about why an indicator appears
- Checking for blanks
- Being able to treate if a zero value is a number or false/blank for numerical fields
- Handles more than 15 unique rulesets per Lighting page
- Handles more than 10 rules per indicator
  - We don't plan to support multiple criteria/conditions at this time, but it's definitely something Conditional Field Formatting has challenged us to think about.  
  - At the same time, because of this, we don't have to worry about users having different experiences other than can they or can't they see an indicator.

While at the time of this post, the team is still working on some items that fill the gaps it has compared to Conditional Field Formatting:

- Our own interface to smoothly setup our version of "rulesets" which are stored in Custom Metadata,
- We have made progress on supporting conditional checks for date fields using actual or relative dates
- Providing more details about the indicator in additional formats
