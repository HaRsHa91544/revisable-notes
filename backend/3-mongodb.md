# MongoDB organizes the data into database -> collection(table) -> document(row).

## A document contain the fields and their respective values where each value is a string, number, array and nested document.

## Why do we need _id?
### Many documents in a collection may contain same fields but to represent a document as unique we need a field which its value is unique for each document.
```js
const product = await Product.findById(req.params.id);
```

## Why do we need schema design?
- MongoDB allows documents in one collection to have different fields.
- To create strictness on what type of data or how data is stored we need a structure to determine. Therefore schema design helps us to make the data predictable.