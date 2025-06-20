# Jira Field Reference

This document serves as a reference for custom field mappings in our Jira instance.

## Custom Field Mappings

| Field Name | Custom Field ID | Description |
|------------|----------------|-------------|
| Acceptance Criteria | customfield_12307 | Contains the acceptance criteria for stories in bullet point format |
| Components | components | Used to categorize issues by system area, e.g., "Backend Processes" |
| Parent | parent | Links stories to their parent epic using the parent field (not epic link) |
| Epic Link (field) | customfield_11406 | Contains the epic key (e.g., "SAF-3271") - auto-populated when parent is set |
| Epic Name (field) | customfield_11405 | Contains the epic summary - auto-populated when parent is set |
| Epic Category | customfield_13568 | Required dropdown field for epics. Common value: "Efficiency / Quality" |

## Usage Example

When updating stories through the Jira API, use the custom field ID to set the acceptance criteria:

```json
{
  "fields": {
    "customfield_12307": "* First acceptance criterion\n* Second acceptance criterion\n* Third acceptance criterion"
  }
}
```

### Creating an Epic

```json
{
  "fields": {
    "summary": "Epic Title",
    "description": "Epic description with h2. headings",
    "issuetype": {"name": "Epic"},
    "project": {"key": "LPM"},
    "customfield_13568": {"value": "Efficiency / Quality"}
  }
}
```

### Linking Stories to Epics

When creating a story as a child of an epic, use both the parent field and the epic link custom field:

```json
{
  "fields": {
    "summary": "Story Title",
    "description": "Story description with user story format",
    "issuetype": {"name": "Story"},
    "project": {"key": "LPM"},
    "parent": "LPM-13456",
    "customfield_11406": "LPM-13456",
    "customfield_12307": "* First acceptance criterion\n* Second acceptance criterion"
  }
}
```

**Important Notes:**
- Use `"parent": "LPM-XXXX"` (string value, not object) when creating stories
- Also set `"customfield_11406": "LPM-XXXX"` to ensure proper epic linking
- The parent field creates the hierarchical relationship
- The epic link custom field ensures visibility in epic views

## Common Operations

### Setting Acceptance Criteria via API

```javascript
// Example of setting acceptance criteria via the Jira API
jira.updateIssue({
  issueKey: "SAF-1234",
  fields: {
    customfield_12307: "* First acceptance criterion\n* Second acceptance criterion"
  }
});
```

## Jira Markup Reference

Since Markdown doesn't work in Jira fields, use Jira's wiki markup language instead:

| Format | Jira Markup | Result |
|--------|------------|--------|
| Headings | h1. Heading 1<br>h2. Heading 2<br>h3. Heading 3 | Large, medium, and small headings |
| Bold | *bold text* | **bold text** |
| Italic | _italic text_ | *italic text* |
| Bullet List | * First item<br>* Second item | • First item<br>• Second item |
| Numbered List | # First item<br># Second item | 1. First item<br>2. Second item |
| Table | \|\|heading 1\|\|heading 2\|\|<br>\|row 1, col 1\|row 1, col 2\| | A simple table with headers |
| Links | [link text\|URL] | Hyperlink with custom text |
| Code Block | {code}<br>code here<br>{code} | Formatted code block |

## Additional Notes

- **Acceptance Criteria Format**: The acceptance criteria field expects bullet points with a leading asterisk (*), each criterion on a new line. No header (like "h2. Acceptance Criteria") should be included in this field.
- **Jira Markup Required**: Markdown formatting **does not work** in Jira fields; use Jira's wiki markup instead (h1., h2., etc.)
- **Field Headers**: Don't include a header that duplicates the field name (e.g., don't add "Description:" at the start of a Description field)
- **Description Format**: For the Description field, include the user story statement first ("As a..."), followed by any background sections using Jira markup
- **Parent Field vs Epic Link**: Use the `parent` field to link stories to epics, not the epic link field. The epic link custom fields are auto-populated when parent is set. For API updates, set both: `"parent": "LPM-XXXX"` (string value) and `"customfield_11406": "LPM-XXXX"` to ensure proper linking. Note that when creating issues via API, the parent field expects a string value (the epic key), not an object with a key property.
- **Epic Category**: Required field for all epics (customfield_13568). Must be set with `{"value": "Category Name"}` format. Common values include "Efficiency / Quality" for tech debt epics.
- **Project Key**: Always use "LPM" as the project key for LPM-related issues
- **Issue Types**: Common issue types include "Story", "Epic", "Bug", "Task"

## Troubleshooting

### Common Issues

| Error | Cause | Solution |
|-------|-------|----------|
| "Epic Category is required" | Missing customfield_13568 when creating Epic | Add `"customfield_13568": {"value": "Efficiency / Quality"}` |
| "Specify a valid 'id' or 'name'" | Incorrect format for select fields | Use `{"value": "Option Name"}` format for dropdown fields |
| "expected 'key' property to be a string" | Using object format for parent field in creation | Use string value: `"parent": "LPM-XXXX"` instead of `"parent": {"key": "LPM-XXXX"}` |
| "Field cannot be set" | Field not available for issue type/screen | Check field availability for the specific issue type |
| Epic link not showing | Parent field set but epic link not populated | Set both parent and customfield_11406 fields |

### Field Validation

- Always validate required fields before API calls
- Check field schemas using the search fields API for complex field structures
- Test with a single issue before batch operations
