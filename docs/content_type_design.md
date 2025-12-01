# Content Type Design

## Overview
A **Content Type** is a user-defined entity definition (e.g., "Product", "Article", "Employee"). It acts as a schema that groups multiple **Fields** together to form a complete record.

The Content Type acts as a central hub. While its primary responsibility is to define the **Data Structure** (Storage), other definitions like **Input Layouts** (Forms) and **Display Views** (Lists/Details) will attach to it.

## JSON Structure
The Content Type is defined by a JSON object containing metadata and a list of fields.

```json
{
  "id": "unique_content_type_id",
  "singularName": "Singular Name",
  "pluralName": "Plural Name",
  "description": "Optional description",
  "fields": [
    // Array of Field Definitions
  ]
}
```

### Properties
| Property | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | Unique identifier (snake_case recommended). Used in API URLs and database collections. |
| `singularName` | `string` | Human-readable singular name (e.g., "Article"). Used for single record views. |
| `pluralName` | `string` | Human-readable plural name (e.g., "Articles"). Used for list views and URL slugs. |
| `description` | `string` | Optional description of the content type's purpose. |
| `fields` | `Array<Field>` | An ordered list of Field objects. See [Data Type Design](data_types_design.md) for the detailed Field structure. |

## Example: Article Content Type
Here is a complete example of how an "Article" content type would be described in JSON.

```json
{
  "id": "article",
  "singularName": "Article",
  "pluralName": "Articles",
  "description": "A standard blog post or news article.",
  "fields": [
    {
      "name": "title",
      "label": "Title",
      "type": "string",
      "isList": false,
      "required": true,
      "unique": true,
      "defaultValue": null,
      "enumOptions": null,
      "targetContentType": null
    },
    {
      "name": "slug",
      "label": "URL Slug",
      "type": "string",
      "isList": false,
      "required": true,
      "unique": true,
      "defaultValue": null,
      "enumOptions": null,
      "targetContentType": null
    },
    {
      "name": "summary",
      "label": "Summary",
      "type": "string",
      "isList": false,
      "required": false,
      "unique": false,
      "defaultValue": null,
      "enumOptions": null,
      "targetContentType": null
    },
    {
      "name": "content",
      "label": "Body Content",
      "type": "string",
      "isList": false,
      "required": true,
      "unique": false,
      "defaultValue": null,
      "enumOptions": null,
      "targetContentType": null
    },
    {
      "name": "author",
      "label": "Author",
      "type": "reference",
      "isList": false,
      "required": true,
      "unique": false,
      "defaultValue": null,
      "enumOptions": null,
      "targetContentType": "person"
    },
    {
      "name": "status",
      "label": "Publish Status",
      "type": "string",
      "isList": false,
      "required": true,
      "unique": false,
      "defaultValue": "draft",
      "enumOptions": [
        { "value": "draft", "label": "Draft" },
        { "value": "published", "label": "Published" },
        { "value": "archived", "label": "Archived" }
      ],
      "targetContentType": null
    },
    {
      "name": "tags",
      "label": "Tags",
      "type": "string",
      "isList": true,
      "required": false,
      "unique": false,
      "defaultValue": [],
      "enumOptions": null,
      "targetContentType": null
    }
  ]
}
```

## System Fields
In addition to the user-defined fields above, every Content Type automatically includes the following system fields. These are managed by the backend and **do not** need to be defined in the JSON.

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | Unique Record ID (UUID). |
| `created_at` | `datetime` | Timestamp when the record was created. |
| `updated_at` | `datetime` | Timestamp when the record was last updated. |
| `created_by` | `reference` | Reference to the User who created the record. |
| `updated_by` | `reference` | Reference to the User who last updated the record. |
