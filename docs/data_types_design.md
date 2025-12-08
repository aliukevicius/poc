# Data Type Design

## Overview
This document defines the structure for **Data Types** and **Fields** within the Dynamic Content Type system.

It follows the **Separation of Concerns** principle:
1.  **Storage**: Defined by `DataType` (primitive structure) and `Field` (nesting & integrity).
2.  **Input**: Defined by `Field` (default values).
3.  **Display**: Defined by [Output Types](./output_types.md) (Widgets & Views).

## 1. Data Type Definition
A `DataType` defines the **primitive storage structure** of a value.
**Crucially, Data Types are system-provided only. No custom or composite Data Types can be created.**

### JSON Structure
```json
{
  "id": "unique_type_id",
  "label": "Human Readable Name",
  "primitiveType": "string" | "number" | "boolean" | "integer"
}
```

### Standard Data Types
These are the core types available out-of-the-box.

| ID          | Primitive Type | Notes                                |
| :---------- | :------------- | :----------------------------------- |
| `string`    | `string`       | Basic text                           |
| `integer`   | `integer`      | Whole numbers                        |
| `float`     | `number`       | Floating point numbers               |
| `boolean`   | `boolean`      | True/False                           |
| `reference` | `string`       | Stores the ID of the referenced item |
| `enum`      | `string`       | Stored one of the pre-defined values |
| `date`      | `string`       | ISO 8601 Date (YYYY-MM-DD)           |
| `time`      | `string`       | ISO 8601 Time (HH:mm:ss)             |
| `datetime`  | `string`       | ISO 8601 Date & Time                 |

---

## 2. Field Definition
A `Field` defines the usage of a `DataType` within a specific **Content Type**.
Fields can be **Simple** (mapping to a primitive Data Type) or **Nested** (grouping other fields).

### Field Types
Fields are now categorized into two conceptual types:
1.  **System Fields**: Pre-configured fields provided by the system (e.g., `media_object`). These typically use a `group` structure behind the scenes but might be presented as a single unit.
2.  **User Fields**: Fields created by the user to structure their content.

### JSON Structure
```json
{
  "name": "field_name",          // Storage Key
  "label": "Field Label",        // Display Label
  "type": "type_id" | "group",   // Reference to DataType OR "group" for nesting
  "isList": boolean,             // Default: false.

  // --- For Nested Fields (type == "group") ---
  "subFields": [                 // Array of Field definitions. REQUIRED if type == "group"
    // Recursive Field Definitions (Currently limited to 1 level of nesting)
  ],

  // --- Storage & Integrity (Primitives) ---
  "required": boolean,           // Default: false
  "unique": boolean,             // Default: false. Enforces uniqueness across the Collection.
  "targetContentType": "id",     // REQUIRED if type == "reference"
  "enumOptions": [               // Optional: Restricts values. ONLY available if type == "enum"
    { "value": "A", "label": "Option A" }
  ],

  // --- Input Configuration ---
  "defaultValue": any,           // Default value for new records
}
```

## Examples

### 1. Simple String Field
```json
{
  "name": "title",
  "label": "Title",
  "type": "string",
  "required": true,
  "unique": true
}
```

### 2. Manual Nested Field (Address)
A user-defined group of fields. Note that sub-fields have the full Field structure.
```json
{
  "name": "address",
  "label": "Address",
  "type": "group",
  "subFields": [
    {
      "name": "street",
      "label": "Street",
      "type": "string",
      "required": true
    },
    {
      "name": "city",
      "label": "City",
      "type": "string",
      "required": true
    },
    {
      "name": "zip",
      "label": "Zip Code",
      "type": "string",
      "required": true
    }
  ]
}
```

### 3. System Preconfigured Field (Currency)
The concepts formerly known as "Composite Data Types" (e.g., Currency) are now just pre-configured nested fields.
```json
{
  "name": "price",
  "label": "Price",
  "type": "group",
  "subFields": [
    {
      "name": "currency",
      "label": "Currency",
      "type": "enum",
      "defaultValue": "USD",
      "enumOptions": [
          { "value": "USD", "label": "US Dollar" },
          { "value": "EUR", "label": "Euro" }
      ]
    },
    {
      "name": "amount",
      "label": "Amount",
      "type": "float",
      "required": true
    }
  ]
}
```

### 4. System Preconfigured Field (Media Asset)
Previously a `media_object` Data Type, now a Nested Field.
```json
{
  "name": "hero_image",
  "label": "Hero Image",
  "type": "group",
  "subFields": [
    { "name": "url", "type": "string", "label": "URL" },
    { "name": "mimeType", "type": "string", "label": "MIME Type" },
    { "name": "size", "type": "integer", "label": "Size" },
    { "name": "altText", "type": "string", "label": "Alt Text" }
  ]
}
```

## Summary of Concerns

| Concept      | Responsibility                    | Example Properties              |
| :----------- | :-------------------------------- | :------------------------------ |
| **DataType** | **Primitive Storage**             | `primitiveType`                 |
| **Field**    | **Structure, Nesting, Integrity** | `type`, `subFields`, `required` |
