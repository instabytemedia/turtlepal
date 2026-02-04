# Entity Relationship Diagram - TurtlePal

> **Auto-generated** from your idea analysis
> **Entities:** 2

---

## Visual Diagram

```mermaid
erDiagram
    profiles {
        uuid id PK
        text username UK
        text display_name
        text avatar_url
        timestamptz created_at
        timestamptz updated_at
    }

    turtlespeciess {
        uuid id PK
        uuid user_id FK
        uuid id
        timestamptz created_at
        timestamptz updated_at
        uuid user_id FK
        text name UK
        text description
        timestamptz created_at
        timestamptz updated_at
    }

    turtleconservationorganizations {
        uuid id PK
        uuid user_id FK
        uuid id
        timestamptz created_at
        timestamptz updated_at
        uuid user_id FK
        text name UK
        text description
        timestamptz created_at
        timestamptz updated_at
    }

    %% Relationships
    profiles ||--o{ turtlespeciess : owns
    profiles ||--o{ turtleconservationorganizations : owns
    turtlespeciess }o--o{ turtleconservationorganizations : "A turtle species can be associated with multiple conservation organizations"
    turtleconservationorganizations }o--o{ turtlespeciess : "A conservation organization can be associated with multiple turtle species"
```

---

## Entity Details

### TurtleSpecies
> A turtle species entity

**Fields:**
  - `id`: uuid (required) - Primary key
  - `created_at`: datetime (required) - Creation timestamp
  - `updated_at`: datetime (required) - Last update timestamp
  - `user_id`: uuid (required) - Owner user ID
  - `name`: string (required, unique, indexed) - Name of the turtle species
  - `description`: text - Description of the turtle species

**Relationships:**
  - many_to_many → **TurtleConservationOrganization**: A turtle species can be associated with multiple conservation organizations

### TurtleConservationOrganization
> A turtle conservation organization entity

**Fields:**
  - `id`: uuid (required) - Primary key
  - `created_at`: datetime (required) - Creation timestamp
  - `updated_at`: datetime (required) - Last update timestamp
  - `user_id`: uuid (required) - Owner user ID
  - `name`: string (required, unique, indexed) - Name of the conservation organization
  - `description`: text - Description of the conservation organization

**Relationships:**
  - many_to_many → **TurtleSpecies**: A conservation organization can be associated with multiple turtle species

---

## Notes

- All entities have standard fields: `id`, `user_id`, `created_at`, `updated_at`
- `PK` = Primary Key, `FK` = Foreign Key, `UK` = Unique Key
- Copy the Mermaid code block to visualize in any Mermaid-compatible tool
- Relationships: `||--o{` = one-to-many, `||--||` = one-to-one, `}o--o{` = many-to-many
