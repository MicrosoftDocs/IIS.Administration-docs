---
uid: api-explorer/interacting
---

# Interacting with Resources

Through the API Explorer resources can be retrieved, updated, created and deleted. The type of interaction is specified by the methods at the top of the navigation panel.

![Api Explorer navigation pane][navigation]

| METHOD |	DESCRIPTION                                    |
|--------|-------------------------------------------------|
| Get	 | Retrieves the specified resource                |
| Post   | Creates a resource                              |
| Patch  | Updates the specified resource                  |
| Delete | Deletes the specified resource                  |
| Head   | Similar to get except only returns HTTP headers |

## Creating a Resource
By selecting the Post option on the navigation panel, resources can be created. When the post option is selected a text area appears on the screen where the JSON representation of a resource can be written. The JSON in this text area is used to create the resource when the Go arrow is clicked.

![Creating a site in the API Explorer][create-site]

## Updating a Resource

The modification of resources is done by issuing patch requests. To update a resource, select the patch option on the navigation pane which will open the text area for the request content. The content of the patch request should contain the desired state of the resource. For example, to stop a web site we send a patch request indicating that the status of the web site should be 'stopped'.

![Stopping a site in the API Explorer][stopping-site]

Here we can see that after a successful patch request the resource reflects the state that we specified.

![Successfully stopped site][stopped-site]

## Deleting a Resource

To delete a resource, first navigate to the target resource in the api explorer. Then select the delete method on the navigation panel and press the go button.


[navigation]: /imgs/navigation.png "The navigation pane of the API Explorer"
[create-site]: /imgs/create-site.png "Creating a site in the API Explorer"
[stopping-site]: /imgs/stopping-site.png "Connecting to the API Explorer"
[stopped-site]: /imgs/stopped-site.png "Browsing with the API Explorer"