# Data Type Design

## Overview
This document defines the structure for **Data Types** and **Fields** within the Dynamic Content Type system.
It follows the **Separation of Concerns** principle:
1.  **Storage**: Defined by `DataType` (structure) and `Field` (integrity/constraints).
2.  **Input**: Defined by `Field` (default values).
3.  **Display**: Defined by Content Type views (list/single) - *to be detailed in a separate design*.

## 1. Data Type Definition
A `DataType` defines the **storage structure** of a value. It does *not* define how it is input or displayed, nor does it define usage-specific constraints (like "required").

### JSON Structure
```json
{
  "id": "unique_type_id",
  "label": "Human Readable Name",
  "storage": "primitive" | "object",

  // IF storage == "primitive"
  "primitiveType": "string" | "number" | "boolean" | "integer",

  // IF storage == "object" (Composite Type)
  "properties": [
    {
      "name": "key",
      "type": "type_id", // Reference to another DataType
      "label": "Property Label"
    }
  ]
}
```

### Standard Data Types
These are the core types available out-of-the-box.

| ID | Storage | Primitive Type | Notes |
| :--- | :--- | :--- | :--- |
| `string` | `primitive` | `string` | Basic text |
| `integer` | `primitive` | `integer` | Whole numbers |
| `float` | `primitive` | `number` | Floating point numbers |
| `boolean` | `primitive` | `boolean` | True/False |
| `reference` | `primitive` | `string` | Stores the ID of the referenced item |

### Composite Type Example: Currency Amount
```json
{
  "id": "currency_amount",
  "label": "Currency Amount",
  "storage": "object",
  "properties": [
    { "name": "currency", "type": "string", "label": "Currency Code" },
    { "name": "amount", "type": "float", "label": "Amount" }
  ]
}
```

---

## 2. Field Definition
A `Field` defines the usage of a `DataType` within a specific **Content Type**. It handles:
1.  **Binding**: Linking a name to a DataType.
2.  **Integrity (Storage)**: Validation rules that ensure data consistency (Required, Unique).
3.  **Input Configuration**: Default values.

### JSON Structure
```json
{
  "name": "field_name",          // Storage Key
  "label": "Field Label",        // Display Label
  "type": "type_id",             // Reference to DataType

  // --- Storage & Integrity ---
  "required": boolean,           // Default: false
  "unique": boolean,             // Default: false
  "targetContentType": "id",     // REQUIRED if type == "reference"

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
  "unique": false,
}
```

### 2. Reference Field
Links to another Content Type (e.g., an Author).
```json
{
  "name": "author_id",
  "label": "Author",
  "type": "reference",
  "targetContentType": "person",
  "required": true,
}
```

### 3. Composite Field (Currency)
Uses the `currency_amount` composite type defined above.
```json
{
  "name": "price",
  "label": "Price",
  "type": "currency_amount",
  "required": true,
  "defaultValue": { "currency": "USD", "amount": 0 },
}
```

## Summary of Concerns

| Concept | Responsibility | Example Properties |
| :--- | :--- | :--- |
| **DataType** | **Storage Structure** | `storage`, `primitiveType`, `properties` |
| **Field** | **Integrity & Usage** | `required`, `unique`, `targetContentType` |
