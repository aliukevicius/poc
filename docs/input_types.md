# Input Types

Input types is all about how the information is inputed by the user for a give content type.

Each content type can have multiple input types. Input types don't need to have all the fields that are defined in the content type, but it must always have the required fields.

Each field in the input type can have validation rules. There rules define if the data for that given field is valid or not. If data is not valid, the user will get an error message.

Each input type will have its own input format. Single input type can support multiple input formats. For example:
- Form data
- form url encoded
- multipart/form-data
- json
- xml
- yaml
- csv
- tsv


Certain input types can allow you to create nultiple records for a given content type at once. This should be configurable. Either that input type supports it or not.
Each input type will also have it dedicated API endpoint.

Each input type belongs to a content type.