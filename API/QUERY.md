# Query

Here we will detail how you can build your own queries to pass into the PayTheory API.

First let's look at a basic query object, then we can break down its parts.

## The Query Object
```graphql
{
    query_list: [
        {
            key: "full_name",
            value: "John Doe",
            operator: EQUAL,
            conjunctive_operator: NONE_NEXT
        }
    ], 
    sort_list: {
        direction: ASC, 
        key: "transfer_date"
    }
}
```

**`query_list`: [QueryPair]**  
A list of query pairs used to build out a query.

**`sort_list`: [SortPair]**  
A list of sort pairs to define how the data should be sorted.

## Query Pair
A query pair is the object to build out a query. There are some required fields and some optional fields.

###### ~ = required

**`key`: String**  
The key to the value you want to query against. 

**`operator`: Operator**  
The operator to use to compare the value to the data you are calling. More detail below.

**`value`: String**  
The value to compare the data to. If using the `LIKE` or `NOT_LIKE` operator, this value can contain [wildcard characters](https://www.w3schools.com/sql/sql_wildcards.asp).

**`in_values`: [String]**  
A list of values to compare the data to. This should be used instead of `value` if using the operators `IN` or `NOT_IN`.

**~`conjunctive_operator`: ConjunctiveOperator**  
The conjunctive operator to use to connect the query pair with the next query pair. More detail below.

**`query_group`: [QueryPairs]**  
A list of query pairs to use to build out a nested query.  
A more detailed example is below under the examples section.

### Operators

These operators are case-sensitive. The following are the available operators:

`EQUAL`  
The data is equal to the value.

`NOT_EQUAL`  
The data is not equal to the value.

`LIKE`  
The data is like the value. The value can contain [wildcard characters](https://www.w3schools.com/sql/sql_wildcards.asp).

`NOT_LIKE`  
The data is not like the value. The value can contain [wildcard characters](https://www.w3schools.com/sql/sql_wildcards.asp).

`IN_LIST`  
The data is in the list of values.

`NOT_IN_LIST`  
The data is not in the list of values.

`GREATER_THAN`  
The data is greater than the value.

`GREATER_EQUAL`  
The data is greater than or equal to the value.

`LESS_THAN`  
The data is less than the value.

`LESS_EQUAL`  
The data is less than or equal to the value.

### Conjunctive Operators

These operators are case-sensitive. Conjunctive operators in the same array must match for a query to work. 
To mix operators use nested queries with query pairs containing a `query_group`. The following are the available conjunctive operators:

`AND_NEXT`  
 The results of the query have to meet all the conditions in the query pair list.

`OR_NEXT`  
 The results of the query have to meet one of the conditions in the query pair list.

`NONE_NEXT`  
 The final query pair in the list should use this operator since it has nothing to connect to.


## Sort Pair
A sort pair is the object used to tell a query how the data should be sorted.

**~`direction`: String**  
The direction to sort the data. These are case-sensitive.
* `ASC`
  * Begins with the least or smallest and ends with the greatest or largest
* `DESC`
  * Begins with the greatest or largest and ends with the least or smallest

**~`key`: String**  
The key to sort the data by.


## Examples

### Settlements With Gross Amount over $10

If you wanted to build a query that looked for any settlements that had a gross_amount over $10 and was sorted by gross_amount in ascending order, you would do the following:

```graphql
{
    settlements(limit: 10, query:
          {
            query_list: [
              {
                key: "gross_amount",
                value: "1000",
                operator: GREATER_THAN,
                conjunctive_operator: NONE_NEXT
              }
            ],
            sort_pair: [{
              direction: ASC,
              key: "gross_amount"
            }]
          }) {
        items {
            currency
            gross_amount
        }
        total_row_count
    }
}
```

### Transactions With Status `SETTLED` and their first name is `John`

If you wanted to build a query that looked for any transactions that had a status of `SETTLED` and had a first name of `John`, you would do the following:

```graphql
{
  transactions(limit: 5, queryTransactionData: {query_list: [
  {
    key: "full_name",
    value: "John%",
    operator: LIKE,
    conjunctive_operator: AND_NEXT
  },
  {
      key: "status",
      value: "SETTLED",
      operator: EQUAL,
      conjunctive_operator: NONE_NEXT
  }
]}) {
    items {
      full_name
      gross_amount
    }
    total_row_count
  }
}
```

### Transactions with a nested query
To build nested queries you can use multiple query pairs with at least one containing a `query_group`.
```graphql
{
  transactions(limit: 5, queryTransactionData: {query_list: [
    {
      query_group: [
        {
          key: "full_name",
          value: "John Doe",
          operator: EQUAL,
          conjunctive_operator: AND_NEXT
        },
        {
          key: "gross_amount",
          value: "100",
          operator: MORE_THAN,
          conjunctive_operator: NONE_NEXT
        }

      ],
      conjunctive_operator: OR_NEXT
    },
    {
      key: "gross_amount",
      value: "1000",
      operator: MORE_THAN,
      conjunctive_operator: NONE_NEXT
    }
  ]}) {
    items {
      full_name
      gross_amount
    }
    total_row_count
  }
}
```

This query would return any transactions where the `full_name` is John Doe and the `gross_amount` is greater than 100 or transactions where the `gross_amount` is greater than 1000.

This allows for more advanced queries and for you to group `AND_NEXT` and `OR_NEXT` in a single query.

### Querying Sub Objects

Due to the fact metadata is a nested data object metadata queries be made by passing a separate array of query pairs for the metadata.
```graphql
{
    transactions(limit: 10, query:
          {
            query_list: [
                {
                    key: "gross_amount",
                    value: "1000",
                    operator: GREATER_THAN,
                    conjunctive_operator: NONE_NEXT
                }
            ],
            sort_pair: [{
              direction: ASC,
              key: "gross_amount"
            }]
          } 
          ) {
        items {
            currency
            gross_amount
            metadata(query_list: [
                {
                    key: "user_defined_payer_id",
                    value: "1234",
                    operator: EQUAL
                }
            ])
        }
        total_row_count
    }
}
```
This would return 10 transactions where the `gross_amount` is greater than 1000 and the payment has metadata `user_defined_payer_id` is equal to 1234. It would be sorted by gross_amount in ascending order.


