# Validation Rules

## Overview
Validation rules ensure data integrity and correctness. The system separates the **definition** of available rules (Registry) from their **usage** (Configuration) in specific fields.

The configuration uses an **Object-based** approach, providing flexibility and extensibility.

---

## 1. Rule Definitions (Registry)
The Registry defines all available validation rules, their applicable data types, and the parameters they accept.

### Definition Schema
Each rule is defined by the following JSON structure:
```json
{
  "id": "rule_id",
  "label": "Human Readable Name",
  "description": "Description of what the rule validates.",
  "applicableTypes": ["string", "integer", ...], // List of DataType IDs or "all"
  "parameters": [
    {
      "name": "param_name",
      "type": "string" | "integer" | "boolean" | "array",
      "description": "Description of the parameter"
    }
  ]
}
```

### Standard Rules Registry

#### Universal Rules
Rules applicable to all data types.

```json
[
  {
    "id": "required",
    "label": "Required",
    "description": "Ensures the field is not null or empty.",
    "applicableTypes": ["all"],
    "parameters": []
  }
]
```

#### String Rules
Rules applicable to `string` types.

```json
[
  {
    "id": "email",
    "label": "Email Format",
    "description": "Validates that the string is a valid email address.",
    "applicableTypes": ["string"],
    "parameters": []
  },
  {
    "id": "min",
    "label": "Minimum Length",
    "description": "Enforces a minimum character count.",
    "applicableTypes": ["string"],
    "parameters": [
      { "name": "length", "type": "integer", "description": "Minimum number of characters" }
    ]
  },
  {
    "id": "max",
    "label": "Maximum Length",
    "description": "Enforces a maximum character count.",
    "applicableTypes": ["string"],
    "parameters": [
      { "name": "length", "type": "integer", "description": "Maximum number of characters" }
    ]
  },
  {
    "id": "regex",
    "label": "Regular Expression",
    "description": "Validates the string against a regex pattern.",
    "applicableTypes": ["string"],
    "parameters": [
      { "name": "pattern", "type": "string", "description": "Regex pattern" },
      { "name": "flags", "type": "string", "description": "Regex flags (optional)" }
    ]
  },
  {
    "id": "in",
    "label": "In List",
    "description": "Restricts the value to a specific set of allowed strings.",
    "applicableTypes": ["string"],
    "parameters": [
      { "name": "values", "type": "array", "description": "List of allowed values" }
    ]
  }
]
```

#### Number Rules
Rules applicable to `integer` and `float` types.

```json
[
  {
    "id": "min",
    "label": "Minimum Value",
    "description": "Enforces a minimum numeric value (inclusive).",
    "applicableTypes": ["integer", "float"],
    "parameters": [
      { "name": "value", "type": "number", "description": "Minimum value" }
    ]
  },
  {
    "id": "max",
    "label": "Maximum Value",
    "description": "Enforces a maximum numeric value (inclusive).",
    "applicableTypes": ["integer", "float"],
    "parameters": [
      { "name": "value", "type": "number", "description": "Maximum value" }
    ]
  }
]
```

#### Array Rules
Rules applicable when `isList: true`.

```json
[
  {
    "id": "min",
    "label": "Minimum Items",
    "description": "Enforces a minimum number of items in the list.",
    "applicableTypes": ["list"],
    "parameters": [
      { "name": "count", "type": "integer", "description": "Minimum number of items" }
    ]
  },
  {
    "id": "max",
    "label": "Maximum Items",
    "description": "Enforces a maximum number of items in the list.",
    "applicableTypes": ["list"],
    "parameters": [
      { "name": "count", "type": "integer", "description": "Maximum number of items" }
    ]
  }
]
```

#### Database Integrity Rules
Rules that involve checking against existing data.

```json
[
  {
    "id": "unique",
    "label": "Unique",
    "description": "Ensures the value is unique across all records of this Content Type.",
    "applicableTypes": ["string", "integer", "float"],
    "parameters": [
      { "name": "table", "type": "string", "description": "Table name (optional)" },
      { "name": "column", "type": "string", "description": "Column name (optional)" }
    ]
  }
]
```

---

## 2. Rule Usage (Configuration)
Validation rules are configured within the `validation` property of a Field Definition or Input Type.
The configuration uses an **array of objects**. Each object specifies the `rule` ID and its `params`.

### Field Definition Example
```json
{
  "name": "username",
  "type": "string",
  "validation": [
    { "rule": "required" },
    { "rule": "unique" },
    { "rule": "min", "params": { "length": 3 } },
    { "rule": "max", "params": { "length": 20 } },
    { "rule": "regex", "params": { "pattern": "^[a-zA-Z0-9_]+$" } }
  ]
}
```

### Input Type Example
```json
{
  "fieldName": "age",
  "validation": [
    { "rule": "required" },
    { "rule": "min", "params": { "value": 18 } },
    { "rule": "max", "params": { "value": 120 } }
  ]
}
```

### Enum Example
```json
{
  "fieldName": "status",
  "validation": [
    { "rule": "required" },
    { "rule": "in", "params": { "values": ["draft", "published", "archived"] } }
  ]
}
```