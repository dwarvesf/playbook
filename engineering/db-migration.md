# Database migration

## Mobile

macOS / iOS:
- Use ObjectMapper cua CoreData de migrate: https://williamboles.me/step-by-step-core-data-migration/

Android
- https://medium.com/flutter-community/migrating-a-mobile-database-in-flutter-sqlite-44ac618e4897

## Backend

- Su dung migration tool tuy ngon ngu
    - Go: sql-migrate
    - Elixir: ecto

## Tips / Best practices checklist

- Luon viet migrate up / down
- back up before migration
- data validation & repair
- Prefer ORM migration tool
- `Do not` deploy the code first: Do not deploy the code first that write to new columns, and then run the migration. It will only result in errors as the code might try to access non-existing column(s).
- Ensure the new changes work before you drop anything
- Use `Feature Flags` whenever possible