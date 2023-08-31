# Database migration practice

> ## Backup Your Data
> To avoid any potential data loss, before you start the migration, make sure you have a backup version of current database so you'll be able to restore if somethings don't go according to the plan.

## Schema Migration
The way you managing the database changes. It could be schema changes (e.g. adding/remove column, table) or data fixes (e.g. change data type of the column from json to jsonb)

### The concept
- Every database changes will be documented incrementally, as small as possible.
- The script should include unique sequence number or timestamp
- Each migration must include up (migrate) and down (rollback).
- Store schema migration history in database
- All migration files are tracked in version control and saved in single place

For example, we want to add indexes to user tables the migration file(s) would be: `20221010113000_add_indexes_to_users.up.sql` and `20221010113000_add_indexes_to_users.down.sql`

### Practices
- Ensure you run the migration before deploying your code.
- For any PR that adds migration before getting merged the new migration version must be the latest
- Never alter old migration files because it has no real effect to the database
- Avoid running auto migration on the production database, auto-run migration is allowed in the dev or staging environment
- Add Guards to scripts: `IF EXISTS` or `IF NOT EXISTS` will be help full most of the case you alter the schema
- Ensure the new changes work before you drop anything. So `DO NOT` drop column, table right away in the change/remove column/table
    - Case of drop: rename old column to `delete_[column_name | table_name]`
    - Case of change column data type: copy value of `old_column` to `new_column`
    - We can remove table/column when we are sure that there's no need to rollback to old data
- Recommended makefile scripts:
    - make migrate-up
    - make migrate-down
- Stick with one migration tool

**Recommended Tools**

- Golang: 
    - [**sql-migrate**](https://github.com/rubenv/sql-migrate)
    - [go-migrate](https://github.com/golang-migrate/migrate)

- Elixir: 
    - [**ecto**](https://hexdocs.pm/ecto_sql/Ecto.Migration.html)

- Nodejs:
    - [Sequelize](https://sequelize.org/docs/v6/other-topics/migrations/)
    - [Typeorm](https://typeorm.io/migrations)


## Data Migration
> Data migration is the process of moving data from one system to another. 

A data migration plan should include consideration of these critical factors:
- Knowing the data — Before migration, source data needs to undergo a complete audit. Unexpected issues can surface if this step is ignored.
- Cleanup — Once you identify any issues with your source data, they must be resolved. This may require additional software tools and third-party resources because of the scale of the work.
- Maintenance and protection — Data undergoes degradation after a period of time, making it unreliable. This means there must be controls in place to maintain data quality.
- Governance — Tracking and reporting on data quality is important because it enables a better understanding of data integrity. The processes and tools used to produce this information should be highly usable and automate functions where possible.


### Migration Strategy
There are 2 type of strategy you can take to build a data migration strategy

**Big Bang Migration**
In a big bang data migration, the full transfer is completed within a limited window of time. Live systems experience downtime while data goes through ETL processing and transitions to the new database.

**Trickle Migration**
Trickle migrations, in contrast, complete the migration process in phases. During implementation, the old system and the new are run in parallel, which eliminates downtime or operational interruptions. Processes running in real-time can keep data continuously migrating.

### Data Migration Best Practices
- Back up the data before executing. Make the backup script for the existing columns' data in the migrate scripts if we want to run the rollback scripts.
- Make sure you must know and understand what you’re migrating, as well as how it fits within the target system.
- Communicate Your Data Migration Process
- Running the test suites after finishing database migration
- Once the implementation has gone live, set up a system to audit the data in order to ensure the accuracy of the migration