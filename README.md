
# Forms And Basic Associations Rails

## Objectives

1. Populate select options based on association options.
2. Assign a FK based on a select box value directly through mass assignment. (post[category_id])
3. Define a belongs_to association writer.
4. Build a form field that will delegate to a belongs_to association writer. (post#category_name=) through controller mass assignment.
5. Define a has_many association writer.
6. Build a form field that will delegate to a has_many association writer. (post#tag_names= - tags won't be unique) through controller mass assignment.

## Notes

were explicitly not teaching nested_attributes or nested ar forms right now.

This is about showing the student the power of custom writers that can handle complex association logic but still work with mass assignment.
