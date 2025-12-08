# Input Types Design

## Overview
**Input Types** define the technical specifications for how data is accepted by the system's API.
This document focuses on the **Transmission** and **Validation** of data, ensuring it conforms to the underlying **Data Type** structure before storage.

> **Note**: User Interface components (Input Widgets) are defined in [Output Types](./output_types.md), as they are considered a form of data presentation (Edit Views).

---

## 1. Input Formats (API)
Input Formats define the serialization methods accepted by the API for creating or updating records.

### Supported Formats
The system supports content negotiation via the `Content-Type` header.

#### 1. JSON (`application/json`)
*   **Usage**: Default format for structured data.
*   **Structure**:
    *   **Structure**:
    *   Keys must match the `field.name` defined in the Content Type.
    *   Values must match the `DataType` primitive (e.g., string, number, boolean).
    *   **Nested Fields** (Groups) are represented as nested JSON objects.
*   **Example**:
    ```json
    {
      "title": "My Article",
      "status": "published",
      "settings": {
        "seo_title": "Best Article",
        "private": false
      }
    }
    ```

#### 2. Multipart Form Data (`multipart/form-data`)
*   **Usage**: Mandatory for **Media Asset** uploads.
*   **Structure**:
    *   Simple fields are sent as form parts.
    *   Files are sent as binary parts with filename metadata.
    *   Nested objects (like `settings`) should use bracket notation (e.g., `settings[seo_title]`) or be sent as a stringified JSON part, depending on implementation preference.

#### 3. CSV / TSV (`text/csv`)
*   **Usage**: Bulk import of flat data.
*   **Limitations**:
    *   Best for simple, flat Content Types.
    *   **Nested Groups** require specific flattening conventions (e.g., `settings.seo_title`).

---

## 2. Validation Enforcement
Validation ensures data integrity. While rules are defined in the **Content Type** (refer to [Validation Rules](./validation_rules.md)), the **Input Type** layer is responsible for enforcing them during the API request lifecycle.

### Validation Process
1.  **Format Check**: Ensure payload matches the `Content-Type` (e.g., valid JSON).
2.  **Type Check**: Ensure values match the target `DataType` (e.g., "123" is a valid string, but `123` is needed for an integer field).
3.  **Constraint Check**: Apply `validation_rules` (e.g., `required`, `min`, `regex`).
4.  **Integrity Check**: Check database constraints (e.g., `unique`, `reference` existence).

### Error Response
If validation fails, the API returns a `422 Unprocessable Entity` with a structured error object.
The structure supports multiple errors per field, as a single value might fail multiple validation rules.
```json
{
  "error": "ValidationError",
  "fields": {
    "email": [
      "Invalid email format",
      "Email domain not allowed"
    ],
    "age": [
      "Must be at least 18"
    ]
  }
}
```

---

## 3. API Endpoints
Standard endpoints for data input:

| Method  | Endpoint                     | Description               | Supported Formats |
| :------ | :--------------------------- | :------------------------ | :---------------- |
| `POST`  | `/api/content/:type_id`      | Create a single record    | JSON, Multipart   |
| `POST`  | `/api/content/:type_id/bulk` | Create multiple records   | JSON Array, CSV   |
| `PUT`   | `/api/content/:type_id/:id`  | Replace a record          | JSON, Multipart   |
| `PATCH` | `/api/content/:type_id/:id`  | Partially update a record | JSON              |