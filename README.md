# ZIS_make_comment_private
This ZIS flow is triggered by a `ticket.CommentAdded`ticket event, it will check that a specific public message "new comment" is authored by the requester, and then make this comment private, so it doesn't appear in public messages on the ticket communication thread. 

This flow is meant to be triggered by a separate webhook triggered from Zendesk Support, authoring the requester message that Activates a Next Reply target on the ticket.

The purpose is to have active SLA next reply-time targets on tickets that require agents to follow up, but technically don't qualify.

# So, we're gaming the system by:

1. Identifying the SLA blind spots and creating triggers for each,
2. These triggers fire on conditions where we want to apply a next reply time target,
3. Their action is to add a public comment to the ticket using an active webhook:
```
{
    "ticket": {
        "comment": {
            "body": "new comment",
			"public": true,
            "author_id": {{ticket.requester.id}}
        }
    }
}
```
4. The ZIS flow listens for this update ☝ and when it happens, it makes that comment private.
