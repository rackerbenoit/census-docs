---
description: This page describes how to use Census with Zendesk.
---

# Zendesk

## Zendesk data quirks

Zendesk does things a little differently so heads up that there are a few gotchas with this integration.

#### Setting multiple organizations on a user

Zendesk optionally allows users to be members of multiple organizations. \(You can find it under: Admin &gt; Customers &gt; Allow users to belong to multiple organizations\).  
  
When creating a User sync in Census, you can provide either a single External ID for the Organization, or a list of External IDs. If providing a list, you'll need to format them as a JSON array of strings, for example:

```text
["123", "example.com"]
```

Census will overwrite any existing organization relationships with the provided value. To maintain a user's existing Organization relationships, make sure that the existing relationships' External IDs appear in your array.

#### Working with tags

Zendesk's tags behave a little differently than other tools in that a ticket may "collect" tags over the course of its life. Users and Organizations are allowed to have tags. A ticket will automatically pick up the tags of a particular user and their org when the ticket is created.  
  
Additionally, when creating new custom dropdown fields or checkboxes, they can be configured to automatically add a tag to the Organization, User, or Ticket.   
  
Because tags are so flexible, we generally suggest you use dropdowns and checkboxes for most Census data sync designs.

However, if you're confident updating Tags directly is the right design for your use case, there's a couple of things to keep in mind:

* Tag names cannot contain spaces, they're treated as separators. 
* Zendesk and Census will work together to create a list of tags from the string you provide. You can pass a JSON array, or a space or comma separated list.
* The tag values you provide will **replace** any existing tags on the object. To maintain existing tags, make sure they appear in the list of tags you provide to Zendesk.

#### Updating a Dropdown or Tagger field

To update a Dropdown in Census, the Zendesk API requires that you provide the API name of the dropdown option value rather than the user-visible label. By default, Zendesk automatically converts any field value to lowercase AND snake\_case, though this can be overridden. This means the value **Paid User** becomes **paid\_user** and the latter is what you'd provide to a Census sync to update the value of a Dropdown. This is an easy way to transform the value in SQL:

```text
lower(replace(column_name, ' ', '_'))
```

If you have any questions, don't hesitate to contact us via live chat [in the dashboard](https://app.getcensus.com/) or emailing us at [support@getcensus.com](mailto:mailto:support@getcensus.com).

