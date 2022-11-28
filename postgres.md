
### Table example

```sql
create table my_name_table(
	id SERIAL PRIMARY KEY,
	name VARCHAR(255),
	age int
);
```

Timeseries

```sql
create table numeric_measurements(
  date timestamp,
  key VARCHAR(255),
  value float4
);
```

### Insert

Insert into timeseries

```sql
INSERT INTO numeric_measurements VALUES(NOW()::timestamp,'temp',50)
```
