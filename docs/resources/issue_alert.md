---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "sentry_issue_alert Resource - terraform-provider-sentry"
subcategory: ""
description: |-
  Sentry Issue Alert resource. Note that there's no public documentation for the values of conditions, filters, and actions. You can either inspect the request payload sent when creating or editing an issue alert on Sentry or inspect Sentry's rules registry in the source code https://github.com/getsentry/sentry/tree/master/src/sentry/rules.
---

# sentry_issue_alert (Resource)

Sentry Issue Alert resource. Note that there's no public documentation for the values of conditions, filters, and actions. You can either inspect the request payload sent when creating or editing an issue alert on Sentry or inspect [Sentry's rules registry in the source code](https://github.com/getsentry/sentry/tree/master/src/sentry/rules).

## Example Usage

```terraform
resource "sentry_issue_alert" "main" {
  organization = sentry_project.main.organization
  project      = sentry_project.main.id
  name         = "My issue alert"

  action_match = "any"
  filter_match = "any"
  frequency    = 30

  conditions = [
    {
      id   = "sentry.rules.conditions.first_seen_event.FirstSeenEventCondition"
      name = "A new issue is created"
    },
    {
      id   = "sentry.rules.conditions.regression_event.RegressionEventCondition"
      name = "The issue changes state from resolved to unresolved"
    },
    {
      id             = "sentry.rules.conditions.event_frequency.EventFrequencyCondition"
      name           = "The issue is seen more than 100 times in 1h"
      value          = 100
      comparisonType = "count"
      interval       = "1h"
    },
    {
      id             = "sentry.rules.conditions.event_frequency.EventUniqueUserFrequencyCondition"
      name           = "The issue is seen by more than 100 users in 1h"
      value          = 100
      comparisonType = "count"
      interval       = "1h"
    },
    {
      id             = "sentry.rules.conditions.event_frequency.EventFrequencyPercentCondition"
      name           = "The issue affects more than 50.0 percent of sessions in 1h"
      value          = 50.0
      comparisonType = "count"
      interval       = "1h"
    },
  ]

  filters = [
    {
      id              = "sentry.rules.filters.age_comparison.AgeComparisonFilter"
      name            = "The issue is older than 10 minute"
      value           = 10
      time            = "minute"
      comparison_type = "older"
    },
    {
      id    = "sentry.rules.filters.issue_occurrences.IssueOccurrencesFilter"
      name  = "The issue has happened at least 10 times"
      value = 10
    },
    {
      id               = "sentry.rules.filters.assigned_to.AssignedToFilter"
      name             = "The issue is assigned to Team"
      targetType       = "Team"
      targetIdentifier = sentry_team.main.team_id
    },
    {
      id   = "sentry.rules.filters.latest_release.LatestReleaseFilter"
      name = "The event is from the latest release"
    },
    {
      id        = "sentry.rules.filters.event_attribute.EventAttributeFilter"
      name      = "The event's message value contains test"
      attribute = "message"
      match     = "co"
      value     = "test"
    },
    {
      id    = "sentry.rules.filters.tagged_event.TaggedEventFilter"
      name  = "The event's tags match test contains test"
      key   = "test"
      match = "co"
      value = "test"
    },
    {
      id    = "sentry.rules.filters.level.LevelFilter"
      name  = "The event's level is equal to fatal"
      match = "eq"
      level = "50"
    }
  ]

  actions = [
    {
      id               = "sentry.mail.actions.NotifyEmailAction"
      name             = "Send a notification to IssueOwners"
      targetType       = "IssueOwners"
      targetIdentifier = ""
    },
    {
      id               = "sentry.mail.actions.NotifyEmailAction"
      name             = "Send a notification to Team"
      targetType       = "Team"
      targetIdentifier = sentry_team.main.team_id
    },
    {
      id   = "sentry.rules.actions.notify_event.NotifyEventAction"
      name = "Send a notification (for all legacy integrations)"
    }
  ]
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `action_match` (String) Trigger actions when an event is captured by Sentry and `any` or `all` of the specified conditions happen.
- `actions` (List of Map of String) List of actions.
- `conditions` (List of Map of String) List of conditions.
- `filter_match` (String) Trigger actions if `all`, `any`, or `none` of the specified filters match.
- `frequency` (Number) Perform actions at most once every `X` minutes for this issue. Defaults to `30`.
- `name` (String) The issue alert name.
- `organization` (String) The slug of the organization the issue alert belongs to.
- `project` (String) The slug of the project to create the issue alert for.

### Optional

- `environment` (String) Perform issue alert in a specific environment.
- `filters` (List of Map of String) List of filters.

### Read-Only

- `id` (String) The ID of this resource.
- `internal_id` (String) The internal ID for this issue alert.
- `projects` (List of String, Deprecated) Use `project` (singular) instead.


