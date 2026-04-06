---
name: atlassian
description: Interact with Jira at nexthink-engineering.atlassian.net — view, create, update, and search issues
user_invocable: true
---

# Atlassian / Jira Skill

## Configuration

- **Base URL:** `https://nexthink-engineering.atlassian.net`
- **User email:** read from `$JIRA_USERNAME` env var (set in `~/.zshrc`)
- **API Token:** read from `$JIRA_API_TOKEN` env var (set in `~/.zshrc`)

All Jira REST API calls use HTTP Basic Auth:

- Username: `$JIRA_USERNAME`
- Password: `$JIRA_API_TOKEN`

Base64 encoded credentials for the `Authorization: Basic <token>` header must be computed at call time using the Bash tool:

```bash
echo -n "$JIRA_USERNAME:$JIRA_API_TOKEN" | base64
```

## How to use this skill

When the user invokes `/atlassian` or asks about Jira issues, use `curl` via the Bash tool to call the Jira REST API v3.

Always compute the auth header inline. Example pattern:

```bash
AUTH=$(echo -n "$JIRA_USERNAME:$JIRA_API_TOKEN" | base64)
curl -s -H "Authorization: Basic $AUTH" -H "Content-Type: application/json" \
  "https://nexthink-engineering.atlassian.net/rest/api/3/issue/EXT-1234" | jq .
```

---

## Common operations

### Get an issue

```bash
GET /rest/api/3/issue/{issueKey}
```

### Search issues (JQL)

```bash
GET /rest/api/3/issue/search?jql=<JQL>&fields=summary,status,assignee,priority&maxResults=20
```

Common JQL examples:

- `project = EXT AND assignee = currentUser() ORDER BY updated DESC`
- `project = EXT AND status != Done AND sprint in openSprints()`
- `key = EXT-8409`

### Create an issue

```bash
POST /rest/api/3/issue
Body: { "fields": { "project": { "key": "EXT" }, "summary": "...", "issuetype": { "name": "Story" }, "description": { "type": "doc", "version": 1, "content": [...] } } }
```

### Update an issue

```bash
PUT /rest/api/3/issue/{issueKey}
Body: { "fields": { "summary": "new summary" } }
```

### Add a comment

```bash
POST /rest/api/3/issue/{issueKey}/comment
Body: { "body": { "type": "doc", "version": 1, "content": [{ "type": "paragraph", "content": [{ "type": "text", "text": "comment text" }] }] } }
```

### Transition an issue (change status)

1. Get available transitions: `GET /rest/api/3/issue/{issueKey}/transitions`
2. Apply: `POST /rest/api/3/issue/{issueKey}/transitions` with `{ "transition": { "id": "<id>" } }`

### Get my profile / account ID

```bash
GET /rest/api/3/myself
```

---

## Instructions

When this skill is invoked:

1. Parse the user's intent (view issue, search, create, update, comment, transition).
2. If an issue key is mentioned (e.g. `EXT-8409`), operate on that issue.
3. Run the appropriate `curl` command via the Bash tool.
4. Parse the JSON response with `jq` and present the relevant fields in a clean, readable format.
5. For issue details, always show: key, summary, status, assignee, priority, reporter, created/updated dates, and description.
6. For search results, show a table: key | summary | status | assignee.
