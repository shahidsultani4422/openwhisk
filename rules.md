---

copyright:
  years: 2017, 2022
lastupdated: "2022-01-12"

keywords: actions, serverless, javascript, node, node.js, functions, triggers, rule

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Associating triggers and actions through rules
{: #rules}

In {{site.data.keyword.openwhisk}}, every time the trigger fires, the rule uses the trigger event as input and invokes the associated action. With the appropriate set of rules, it's possible for a single trigger to invoke multiple actions, or for an action to be invoked as a response to events from multiple triggers.
{: shortdesc}

## Creating a rule from the console
{: #rules_ui}

From the console, the rule to associate an action and a trigger is created for you.
{: shortdesc}

When you create or access the details for an action or trigger, you have the option of connecting an existing or a new action or trigger. When you make the connection, a rule is created for you and is named in the format `ACTION_NAME-TRIGGER_NAME`.

From the CLI, you can run `ibmcloud fn rule list` to verify that the rule was created for you.

## Creating rules from the CLI
{: #rules_create}

Rules are used to associate a trigger with an action. Each time a trigger event is fired, the action is invoked with the parameters from the trigger event.

Before you begin, create [an action](/docs/openwhisk?topic=openwhisk-actions) and [a trigger](/docs/openwhisk?topic=openwhisk-triggers).

Create a rule to associate a trigger with an action. Rules must be created directly within a namespace and can't be created inside packages.

```sh
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


To disable the rule, you can run the following command.

```sh
ibmcloud fn rule disable RULE_NAME
```
{: pre}

## Creating rules for action sequences
{: #rules_seq}

You can use rules to associate triggers with action sequences.

Before you begin, create [an action sequence](/docs/openwhisk?topic=openwhisk-sequences) and [a trigger](/docs/openwhisk?topic=openwhisk-triggers).

```sh
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}

## Associating multiple triggers and actions
{: #rules_assoc}

You can use different combinations of triggers and actions by creating a rule for each combination. You are not required to have a one to one ratio for actions and triggers.

For example, consider the following actions.

| Action | Description |
| --- | --- |
| `classifyImage` | An action that detects the objects in an image and classifies them. |
| `thumbnailImage` | An action that creates a thumbnail version of an image. |
{: caption="Table 1. Actions and descriptions" caption-side="bottom"}

Also, suppose that two event sources are firing the following triggers.

| Trigger | Description |
| --- | --- |
| `newTweet` | A trigger that is fired when a new tweet is posted. |
| `imageUpload` | A trigger that is fired when an image is uploaded to a website. |
{: caption="Table 2. Event triggers and descriptions" caption-side="bottom"}

You can set up rules so that a single trigger event invokes multiple actions, and have multiple triggers invoke the same action.
- `newTweet -> classifyImage` rule
- `imageUpload -> classifyImage` rule
- `imageUpload -> thumbnailImage` rule

The three rules establish the following behavior.
- Images in both tweets are classified.
- Uploaded images are classified
- A thumbnail version is generated.


