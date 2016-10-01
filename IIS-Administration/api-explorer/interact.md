---
uid: api-explorer/interact.md
---

# Interacting with resources

The capability of the API Explorer is not limited to browsing. Resources can be updated, created and deleted directly from the explorer. The type of interaction is specified by the methods at the top of the navigation panel.

![Api Explorer navigation pane][navigation]


| Method | description                                     |
|--------|-------------------------------------------------|
| Get    | Retrieves the specified resource                |
| Post   | Creates a resource                              |
| Patch  | Updates the specified resource                  |
| Delete | Deletes the specified resource                  |
| Head   | Similar to get except only returns HTTP headers |


## Creating a Resource

By selecting the Post option on the navigation panel resources can be created. When the post option is selected a text area appears on the screen where the JSON representation of a resource can be written. The JSON in this text area is used to create the resource when the Go arrow is clicked.

![Creating a site in the API Explorer][create-site]

## Deleting a Resource

To delete a resource navigate to the target resource in the api explorer. Select the delete method on the navigation panel and press the go arrow.


[navigation]: /imgs/navigation.png "Api Explorer navigation pane"
[create-site]: /imgs/create-site.png "Creating a site in the API Explorer"