# Some useful MySQL commands

### Duplicating a MySQL table, indices, and data

\
To copy just structure and data use this one:

```
CREATE TABLE new_table AS SELECT * FROM old_table;
```

\
To copy with indexes and triggers do these 2 queries:

```
CREATE TABLE new_table LIKE old_table;
```

```
INSERT INTO new_table SELECT * FROM old_table;
```