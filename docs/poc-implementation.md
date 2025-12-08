# POC Implementation

The goal of the POC is to verify how all the data type ideas work in practice.

Because we are trying to verify the ideas, we don't need a lot of things that a regular application would need.

# Stack

- HTML
- JavaScript
- CSS

For the database we will use local storage.


The whole implementation will be browser base. So couple of HTML files and JavaScript files.


# POC project description

We are building a small part of CRM. It will have two content types called "Person" and "Organization".
The goal is to check how various data types, input widgets, and complex data types work in practice.

## Data types

### Person
- First Name
- Last Name
- Contact channels:
  - Emails
  - Phones
- Birth Date
- Gender
- Marital Status
- Organizations

### Organization
- Name

## Input types

Input types should also include input validation.

- Organization (full) form input
- Organization (full) API REST input
- Person (full) form input
- Person (full) API REST input
- Person(partial) API REST input: first name, last name
- Person(partial) Form input: first name, last name, gender


## Output types

- Organization (full record) form output
- Organization (full record) HTML output
- Organization (full record) API REST output
- Organization (list) HTML output
- Organization (list) API REST output

- Person (full record) form output
- Person (full record) HTML output
- Person (full record) API REST output
- Person(partial record) API REST output: first name, last name
- Person(partial record) HTML output: first name, last name, gender
- Person(partial record) Form output: first name, last name, gender
- Person (list) HTML output
- Person (list) API REST output
- Person (list partial) API REST output: first name, last name
- Person (list partial) HTML output: first name, last name


# POC goals

- [ ] Verify that the system data type ideas work in practice.
- [ ] Verify that the system validation rules work in practice.
  - [ ] Definition
  - [ ] Ability to run the validation rules
- [ ] Verify that the content type idea works in practice.
- [ ] Verify that the input type idea works in practice.
  - [ ] For form submit
  - [ ] For API REST submit
- [ ] Verify that the output type idea works in practice.
  - [ ] For form display
  - [ ] For API REST display
  - [ ] For single record display
  - [ ] For list display
  - [ ] Verify that the widget idea works in practice.
