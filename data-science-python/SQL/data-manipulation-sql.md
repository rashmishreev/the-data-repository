## Subqueries

A query nested inside another query. Powerful for performing complex filters and transformations.
Can be placed in a `SELECT`, `FROM`, `WHERE`, `GROUP BY`

Can return a variety of information:
- Scalar quantities (3.14159, -2, 0.001)
- A list (id = (12, 25, 392, 401, 939))
- A table

Why subqueries?
Comparing groups to summarized vaues
- How did Liverpool compare to the English Premier League's average performance for that year?
Reshaping data
- What is the highest monthly avaerage of goals scored in the Bundesliga?
Combinbing data that cannot be joined
- How do you get both the home and away team names into a table of match results?

## Correlated subquery

Correlated subqueries are a special kind of subquery that use values from the outer query in order to generate the final results. 

The subquery is re-executed each time a new row in the final data set is returned, in order to properly generate each new piece of information. 

Correlated subqueries are used for special types of calculations, such as advanced joining, filtering, and evaluating of data in the database.


## Nested Subquery

Subqueries nested inside other subqueries.