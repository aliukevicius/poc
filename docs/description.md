# Project description

We are designing data structures for dynamic content types. This is just a small part of a bigger project, but a very important one.

The bigger project is about building CRM where customers can configure their own data types. It is just a good to know that the bigger project is about building CRM, but the focus for now is on the dynamic content types.

## What are dynamic content types?

Dynamic content types are the content types that are defined at runtime/configuration time rather than hardcoded. The whole description of dynamic content type is stored in a JSON format.

Each dynamic content type is combined from fields/attributes. Each field has a type. Type basically describes how data will be stored.


## Separation of concerns

When designing the data structure for dynamic content types, we must strictly separate concerns on the backend, while acknowledging that the end-user often views them as a single unified concept.

### The Backend Reality: Strict Separation
On the technical side, we distinguish between three core concerns:
1. **Data Storage**: How data is persisted. This includes the underlying data structure, data integrity, unique constraints, and required fields.
2. **Data Display**: How data is presented. This covers list views, single record views, and format (JSON, HTML, etc.). It also includes configuration for API endpoints, search, filtering, and sorting.
3. **Data Input**: How data is collected. This involves forms, file uploads, API ingestion, and input validation.

*Note: Data integrity validation (e.g., unique constraints) belongs to Storage, while input validation (e.g., valid email format) belongs to Input.*

### The User Perspective: The "Widget"
From an end-user's perspective, these concerns are often invisible. They think in terms of a **"Field"** or a **"Widget"**.
- To a user, a "Currency Field" is a single thing that stores money, shows a currency symbol, and lets them type numbers.
- **We package these multiple concerns into a single "Widget" for the user.**
- The Widget is the configuration layer that bundles the Backend Storage, Display logic, and Input forms into a cohesive experience for the administrator configuring the system.




## Requirements

1. Content type should be described via JSON
   1. Each content type is combined from fields/attributes
2. Field/attribute is described by a JSON
   1. A field has a type. Types should be standard like:
      1. Boolean
      2. Int
      3. Number
      4. string
   2. In the future we may also have composite types where a type is composed from multiple fields.
   3. A field can also be a reference to another content type. In this case it is a reference field.
   4. A field may have a default value.


