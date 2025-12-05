# Output Types & Widgets Design

## Overview
**Output Types** define how data is **presented** to the user or external systems. This encompasses both:
1.  **Read Views**: How data is displayed (e.g., Tables, Details, JSON responses).
2.  **Write Views**: How data is edited (e.g., Forms, Input Widgets).

By treating "Input Widgets" as a form of Output (specifically, the output of an "Edit Form"), we centralize all UI/Presentation logic here, keeping `input_types.md` focused purely on data transmission.

---

## 1. Widgets (UI Components)
Widgets are the building blocks of the UI. They are bound to **Data Types** but can vary based on the context (Read vs. Write).

### A. Form Widgets (Write View)
These are components used to *input* or *edit* data.

| Data Type   | Default Widget    | Alternative Widgets                                     | Configuration Options              |
| :---------- | :---------------- | :------------------------------------------------------ | :--------------------------------- |
| `string`    | `text_input`      | `textarea`, `rich_text`, `email`, `url`, `color_picker` | `placeholder`, `maxLength`, `rows` |
| `integer`   | `number_input`    | `slider`, `rating`                                      | `min`, `max`, `step`               |
| `float`     | `number_input`    | `slider`                                                | `min`, `max`, `step`, `precision`  |
| `boolean`   | `checkbox`        | `switch`, `toggle_button`                               | `label_on`, `label_off`            |
| `date`      | `date_picker`     | `text_input` (masked)                                   | `format`, `minDate`, `maxDate`     |
| `time`      | `time_picker`     | `text_input` (masked)                                   | `format`, `minTime`, `maxTime`     |
| `datetime`  | `datetime_picker` | `text_input` (masked)                                   | `format`                           |
| `reference` | `select_dropdown` | `autocomplete`, `radio_group`                           | `searchable`, `multi_select`       |
| `enum`      | `select_dropdown` | `radio_group`, `button_group`                           | -                                  |

### B. Display Widgets (Read View)
These are components used to *view* data.

| Data Type   | Default Widget    | Alternative Widgets                  | Configuration Options                 |
| :---------- | :---------------- | :----------------------------------- | :------------------------------------ |
| `any`       | `field_value`     | -                                    | `label_position` (top/left)           |
| `json`      | `json_viewer`     | `code_block`                         | `collapsed_depth`, `theme`            |
| `string`    | `text_view`       | `code_block`, `markdown_view`        | `lines` (truncate)                    |
| `boolean`   | `icon_status`     | `yes_no_text`, `badge`               | `icon_true`, `icon_false`             |
| `date`      | `formatted_date`  | `relative_time` (e.g., "2 days ago") | `format_string`                       |
| `reference` | `link_chip`       | `card_preview`                       | `display_field` (which field to show) |
| `media`     | `image_thumbnail` | `file_download_link`, `video_player` | `width`, `height`                     |

### C. Widget Details & Configuration

#### 1. Field Value (`field_value`)
The standard "Label + Value" display.
*   **Usage**: Default presentation for most properties in a Details View.
*   **Config**:
    *   `showLabel`: boolean (default: true)
    *   `labelPosition`: "left" | "top" (default: "left")

#### 2. JSON Viewer (`json_viewer`)
Interactive tree view for complex objects or list data.
*   **Usage**: Debugging, displaying raw API responses, or complex composite types.
*   **Config**:
    *   `initialExpandDepth`: integer (default: 1)
    *   `enableClipboard`: boolean (default: true)

#### 3. Code Block (`code_block`)
Displays text in a monospaced, syntax-highlighted block.
*   **Usage**: Displaying code snippets, logs, or configuration files.
*   **Config**:
    *   `language`: string (e.g., "javascript", "json", "python")
    *   `showLineNumbers`: boolean

---

## 2. Output Formats (API)
Output Formats define how the API serializes data for consumption by clients.

### Supported Formats
1.  **JSON (`application/json`)**
    *   **Standard**: Returns raw data matching the Data Type structure.
    *   **Usage**: SPA frontends, mobile apps, external integrations.

2.  **HTML (`text/html`)**
    *   **Server-Side Rendering**: Returns fully rendered HTML pages or fragments.
    *   **Usage**: Traditional web views, SEO-friendly pages.

3.  **CSV (`text/csv`)**
    *   **Usage**: Data export for reporting/spreadsheets.

4.  **PDF (`application/pdf`)**
    *   **Usage**: Printable reports, invoices.

---

## 3. View Definitions
A **View** is a configuration that groups fields and assigns widgets for a specific purpose.

### Example: Edit Form View
```json
{
  "id": "edit_article",
  "type": "form",
  "contentType": "article",
  "layout": [
    {
      "field": "title",
      "widget": {
        "type": "text_input",
        "config": {
          "placeholder": "Enter article title",
          "mask": "AA-9999" // Example custom mask
        }
      }
    },
    {
      "field": "status",
      "widget": {
        "type": "select_dropdown",
        "config": {
          "options": [
            { "value": "draft", "label": "Draft" },
            { "value": "published", "label": "Published" }
          ]
        }
      }
    },
    {
      "field": "content",
      "widget": {
        "type": "rich_text_editor",
        "config": {
          "toolbar": ["bold", "italic", "link", "image"],
          "minHeight": "300px"
        }
      }
    }
  ]
}
```

### Example: List View (Table)
```json
{
  "id": "list_articles",
  "type": "table",
  "contentType": "article",
  "columns": [
    {
      "field": "title",
      "widget": { "type": "link" }
    },
    {
      "field": "status",
      "widget": {
        "type": "badge",
        "config": {
          "colorMap": { "draft": "gray", "published": "green" }
        }
      }
    },
    {
      "field": "publishDate",
      "widget": {
        "type": "formatted_date",
        "config": { "format": "YYYY-MM-DD" }
      }
    }
  ]
}
```
