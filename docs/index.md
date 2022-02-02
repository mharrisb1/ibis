<figure markdown> 
  ![Image title](/static/img/ibis_sky.png){ width="300" }
  <figcaption>Write your analytics code once, run it everywhere.</figcaption>
</figure>

## Features

Ibis provides a standard way to write analytics code, that then can be run in
multiple engines.

- **Full coverage of SQL features**: Anything you can write in a `SELECT` statement you can wrtite in Ibis
- **Abstract over SQL differences**: Write standard code that translates to any SQL syntax
- **High performance execution**: Execute at the speed of your backend, not your local computer
- **Integration with community infrastructure**: Ibis works with existing Python tools

## Supported Backends

- Traditional DBMSs: [PostgreSQL](/backends/postgres), [MySQL](/backends/mysql), [SQLite](/backends/sqlite)
- Analytical DBMSs: [OmniSciDB](/backends/omnisci), [ClickHouse](/backends/clickhouse), [Datafusion](/backends/datafusion)
- Distributed DBMSs: [Impala](/backends/impala), [PySpark](/backends/pyspark), [BigQuery](/backends/bigquery)
- In memory analytics: [pandas](/backends/pandas), [Dask](/backends/dask)

## Example

The next example is all the code you need to connect to a database with a
countries database, and compute the number of citizens per squared kilometer in Asia:

```python
>>> import ibis
>>> db = ibis.sqlite.connect('geography.db')
>>> countries = db.table('countries')
>>> asian_countries = countries.filter(countries['continent'] == 'AS')
>>> density_in_asia = asian_countries['population'].sum() / asian_countries['area_km2'].sum()
>>> density_in_asia.execute()
130.7019141926602
```

Learn more about Ibis in [our tutorial](/tutorial/01-Introduction-to-Ibis).

## Comparison to other tools

!!! tip "Coming from SQL?"

    Check out [Ibis for SQL Programmers](/user_guide/sql)!

### Why not use [pandas](https://pandas.pydata.org/)?

pandas is great for many use cases. But pandas loads the data into the
memory of the local host, and performs the computations on it.

Ibis instead, leaves the data in its storage, and performs the computations
there. This means that even if your data is distributed, or it requires
GPU accelarated speed, Ibis code will be able to benefit from your storage
capabilities.

### Why not use SQL?

SQL is widely used and very convenient when writing simple queries. But as
the complexity of operations grow, SQL can become very difficult to deal with.

With Ibis, you can take fully advantage of software engineering techniques to
keep your code readable and maintainable, while writing very complex analytics
code.

### Why not use [SQLAlchemy](https://www.sqlalchemy.org/)?

SQLAlchemy is very convenient as an ORM (Object Relational Mapper), providing
a Python interface to SQL databases. Ibis uses SQLAlchemy internally, but aims
to provide a friendlier syntax for analytics code. And Ibis is also not limited
to SQL databases, but also can connect to distributed platforms and in-memory
representations.

### Why not use [Dask](https://dask.org/)?

Dask provides advanced parallelism, and can distribute pandas jobs. Ibis can
process data in a similar way, but for a different number of backends. For
example, given a Spark cluster, Ibis allows to perform analytics using it,
with a familiar Python syntax. Ibis supports Dask as a backend.