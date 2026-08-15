# __machine_account_playground

## Tokens

### Org owner gate check
#### token (e.g. fine grained PAT)
Organization permissions: Read access to members

### Main functionality
#### Machine account token (Github classic Token)
Tested with:  

```
repoFull control of private repositories
repo:statusAccess commit status
repo_deploymentAccess deployment status
public_repoAccess public repositories
repo:inviteAccess repository invitations
security_eventsRead and write security events
```

and also (probably unnecessary but for completeness):

```
workflow Update GitHub Action workflows

admin:repo_hookFull control of repository hooks
write:repo_hookWrite repository hooks
read:repo_hookRead repository hooks

admin:org_hookFull control of organization hooks

userUpdate ALL user data
read:userRead ALL user profile data
user:emailAccess user email addresses (read-only)
user:followFollow and unfollow users

delete_repoDelete repositories
```

###### Note - a Fine Grained PAT will not allow PRs to be raised

According to Gemini:

This is a known architectural design limitation of Fine-Grained Personal Access Tokens (PATs) on GitHub.

Why the Fine-Grained PAT Fails with 403
When raising a pull request across forks:

The API call is sent to the target parent repository (POST /repos/PARENT_OWNER/TARGET_REPO/pulls).

GitHub requires the token to have pull_requests: write permission on PARENT_OWNER/TARGET_REPO.

However, a Fine-Grained PAT cannot be granted permissions on repositories owned by a different user/account unless that account is an Organization that has specifically added the machine account as a member or collaborator.

Because the machine account's Fine-Grained PAT is scoped exclusively to repositories owned by or accessible within the machine user's account boundary, GitHub rejects the API request when attempting to act against a personal account owned by PARENT_OWNER.

Why Classic PATs Work
Classic PATs (repo scope) do not enforce repository-level resource boundaries in the same way. A Classic PAT with repo scope acts with the full identity permissions of the user account that created it. As long as a public parent repository exists, any user (and thus any Classic PAT) is permitted to open a PR against it.