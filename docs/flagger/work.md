---
id: work
title: How Does Airship Work?
sidebar_label: How Does Airship Work?
---

At the highest level, Airship routes data & traffic to features.

Airship also enables you to get specific of the data & traffic as well as which features are presented.

Airship consists of:
- Dashboard UI for authoring and managing flags and re-routing traffic
- Server that notifies live clients (devices, browsers, etc.) of changes to any flags and updates applications
- Flagger SDKs which are installable clients that communicate with Airship servers and has an API for developers to incorporate flags


## What is a flag?

If you’re familiar with coding your app with feature switches, then Airship’s flags will make your life infinitely easier (not to mention more powerful).

Let’s say you are developing an accounting app with a Billing feature that generates invoices monthly. You now want to try a new Billing feature that generates invoices weekly.

Traditionally, one way to do this would be to deploy a pilot or beta version of your app. Each deployment would serve up that feature.

![users](assets/users.jpeg)

The issues with this are clear—it is very rigid. Dependencies are easily hidden. There is no certainly if the features are merged what will happen from a QA perspective, or what additional bugs may arise.

If you were savvy, you might create a code switch to handle the traffic in some way.

![](assets/code-switch.jpeg)
 
This brings the features into the same codebase. However, this logic can only be updated within the code and with another deployment. Additionally, if more features are added, this logic becomes increasingly complex and fractured.

Airship sits between your traffic (or content, or other entity interacting with your app) and gives you immediate and virtual control over who sees what feature.
You can configure and access multiple flags in a project and enables you to do so much more:


- Turn your feature on or off without redeploying
- Add variations to your feature
- Direct subsets of your users or entities to different feature variations
- Direct specific individuals or entities to specific feature variations
- Prevent specific individuals or entities to specific feature variations
- Preview how your traffic population will flow through your feature variations as you configure your flag

## What is an entity?

An entity is something that can touch your software. Generically, if it’s a noun, it’s an entity. The most common entity is a user, but other things can be entities as well. Here are a few examples:

- Content
- Addresses
- Organizations
- Locations
- Departments
- Product items

When you define entities (section: how do I create entities?), you enable Airship to serve them through a flag. What entities you define and what flags to send them through is the main function of Airship.

Entities are minimally comprised of:


- An ID (required)
    - This is a unique identifier you specify for your purposes. It can be any alphanumeric string.
- A type (required)
    - This is the “noun” description (above) that you define to categorize your entities.
- A Name (optional, but recommended)
    - This is a human-readable identifier for the entity. While optional, Airship is designed to display this whenever entities are listed for easy recognition.

Entities are also allowed an unlimited number of user-defined fields. These can be helpful in the management and organization of your flag results, but they do not impact the operation or results of Airship. These fields are fully readable within Airship in the Entities section any project.

Entities are unique to each environment (link to topic of environments).

## How do flags and variations work?

The default flag allows you to turn the feature on or off. This alone is a timesaver from a redeployment and stability perspective.

Let’s say you are developing an accounting app with a Billing feature.
The default flag would allow you to turn this feature on or off. All entities would either have access to the feature (on), or all entities would not have access to the feature (off).

An advanced flag allows you to direct entities through different variations of the feature. 

![](assets/variation-flag-on.jpeg)
![](assets/variation-flag-off.jpeg)

But let’s say there are two ways payroll could run—monthly or weekly. These two variations allow some of your entities to receive the weekly variation of the feature and some receive the monthly variation. Some in the above examples is split evenly 50% by default (this split is adjustable). So while the feature is turned on, your entities will be evenly divided between the two features. As you would expect, you could still turn the entire feature off.

The advanced flag can technically receive an unlimited number of variations, but practicality will keep it to a handful. This means you could have payroll variations of quarterly, monthly, bi-weekly, weekly, daily, or even June, Monday, and 1pm. The possibilities are flexible to your solution and endless.

## How do entities relate to flags?

For any given flag you must define the entities that will flow through it.

Airship flags are designed to receive entities by type. By default, the User type (if detected in your entities population) is connected to your flag. This means that all user entities will receive your feature if it is turned on, or all user entities will not receive your feature if it is turned off.

You may add an unlimited number of entity types to the flag.

## What is an entity subset?

Once an entity type is added to your flag, all entities will receive the feature (or a variation) when turned on. 

The most basic control over an entity is percentage. This determines how many entities of the entire pool of that type will have access to this flag.

To limit this pool by certain attributes, you may add filters. Doing so creates an entity subset. As you add filters, you will see a preview of the estimated number of matching entities.

![](assets/entity-subset.jpeg)

You may add an unlimited number of filters. Of course, doing so might reduce your number of matching entities to zero.

## How do entity subset filters work?

Filters are comprised of three parts: an attribute, an operator, and a value. The attributes and values are provided by your dataset \[<— making this up] and are contextual to the entity type.

For example, a user entity subset might be filtered where “operating system is not MacOS.” In this example, it may be that my original pool of user entities was 3,000. When this filter is applied it may reduce it to 1,200.

There is no limit to the number of filters you may add to an entity subset.
Once entities are filtered into a subset, it makes sense you could add the same subset to the flag and filter it differently.

![](assets/entity-subset-work.jpeg)

MacOS,” you could add another user entity subset and filter it where “state is California.” This would give you user entities who did not have an operating system of MasOS regardless what state they have and users entities who have a state of California regardless of what operating system they have. 

In this example, it is very likely there is a user who will appear in both pools. However, Airship will only serve them up once an make note of the duplicate.

## What is whitelisting and blacklisting?

Typically, entities flow from their subsets through the flag variations. They are assigned to variations randomly based on the percentages assigned (either evenly split by default or by your custom percentages). This is useful in most cases. However, there may be certain entities you wish to assign to a specific variation.

![](assets/wl-bl.jpeg)

You can assign any entity to a specific variation and they will see that variation (as long as the flag is on). You can also assign any entity directly to the constant _Off_. In that case, they will never see any variation regardless of the flag being on or off.

## Flagging Rules
- Order of precedence
