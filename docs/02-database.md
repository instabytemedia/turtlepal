# 02 - Database Schema

> **Agent:** Database Agent 🗄️
> **Goal:** Set up Supabase database with all tables and policies

---

## ⚠️ CRITICAL: Avoid Duplicate Fields

**NEVER define these fields in your custom schema - they are automatically added:**
- `id` - UUID Primary Key (auto-generated)
- `user_id` - UUID Foreign Key to auth.users (auto-generated)
- `created_at` - TIMESTAMPTZ (auto-generated)
- `updated_at` - TIMESTAMPTZ (auto-generated)

**Only define entity-SPECIFIC fields in your tables.**

Example of WRONG vs RIGHT:
```sql
-- WRONG: Duplicate fields!
CREATE TABLE items (
  id UUID PRIMARY KEY,       -- Already auto-added!
  user_id UUID,              -- Already auto-added!
  created_at TIMESTAMPTZ,    -- Already auto-added!
  name TEXT,
  id UUID,                   -- DUPLICATE! Will cause error!
  created_at TIMESTAMPTZ     -- DUPLICATE! Will cause error!
);

-- RIGHT: Only custom fields
CREATE TABLE items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  -- Only custom fields below:
  name TEXT NOT NULL,
  description TEXT,
  status TEXT NOT NULL DEFAULT 'active'
);
```

---

## Context

**Product:** TurtlePal
**Entities:** TurtleSpecies, TurtleConservationOrganization

### Data Model

**TurtleSpecies**: A turtle species entity
- `id`: UUID (required) - Primary key
- `created_at`: TIMESTAMPTZ (required) - Creation timestamp
- `updated_at`: TIMESTAMPTZ (required) - Last update timestamp
- `user_id`: UUID (required) - Owner user ID
- `name`: TEXT (required, unique, indexed) - Name of the turtle species
- `description`: TEXT - Description of the turtle species
Relationships:
  - many_to_many → TurtleConservationOrganization: A turtle species can be associated with multiple conservation organizations

**TurtleConservationOrganization**: A turtle conservation organization entity
- `id`: UUID (required) - Primary key
- `created_at`: TIMESTAMPTZ (required) - Creation timestamp
- `updated_at`: TIMESTAMPTZ (required) - Last update timestamp
- `user_id`: UUID (required) - Owner user ID
- `name`: TEXT (required, unique, indexed) - Name of the conservation organization
- `description`: TEXT - Description of the conservation organization
Relationships:
  - many_to_many → TurtleSpecies: A conservation organization can be associated with multiple turtle species

---

## Instructions

### 1. Profiles Table

Create a **profiles** table that syncs with auth.users:

**Requirements:**
- Primary Key: `id` (UUID) - Reference to auth.users(id)
- Fields: username (unique), display_name, avatar_url, bio
- Timestamps: created_at, updated_at
- RLS: Everyone can view profiles, only owner can update
- Auto-Trigger: Automatically create profile on user signup

**Important:**
- ON DELETE CASCADE for foreign key to auth.users
- Trigger Function for auto-create on signup
- Public read access, owner-only write access

### 2. Entity Tables

Create a table for each entity:

**1. TurtleSpecies Table (`turtlespeciess`):**
Purpose: A turtle species entity

```sql
CREATE TABLE turtlespeciess (
  -- Standard fields
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

  -- Entity-specific fields
  name TEXT NOT NULL UNIQUE -- Name of the turtle species,
  description TEXT -- Description of the turtle species

);

-- Indexes for performance
CREATE INDEX turtlespeciess_user_id_idx ON turtlespeciess(user_id);
CREATE INDEX turtlespeciess_created_at_idx ON turtlespeciess(created_at DESC);
CREATE INDEX turtlespeciess_name_idx ON turtlespeciess(name);


-- RLS Policies (Granular)
ALTER TABLE turtlespeciess ENABLE ROW LEVEL SECURITY;

-- SELECT: Users can only view their own records
CREATE POLICY "turtlespeciess_select_own"
  ON turtlespeciess FOR SELECT
  USING (auth.uid() = user_id);

-- INSERT: Users can only create records for themselves
CREATE POLICY "turtlespeciess_insert_own"
  ON turtlespeciess FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- UPDATE: Users can only update their own records
CREATE POLICY "turtlespeciess_update_own"
  ON turtlespeciess FOR UPDATE
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);

-- DELETE: Users can only delete their own records
CREATE POLICY "turtlespeciess_delete_own"
  ON turtlespeciess FOR DELETE
  USING (auth.uid() = user_id);
```

Relationships:
- many_to_many to TurtleConservationOrganization (foreign key: turtleconservationorganization_id)

**2. TurtleConservationOrganization Table (`turtleconservationorganizations`):**
Purpose: A turtle conservation organization entity

```sql
CREATE TABLE turtleconservationorganizations (
  -- Standard fields
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

  -- Entity-specific fields
  name TEXT NOT NULL UNIQUE -- Name of the conservation organization,
  description TEXT -- Description of the conservation organization

);

-- Indexes for performance
CREATE INDEX turtleconservationorganizations_user_id_idx ON turtleconservationorganizations(user_id);
CREATE INDEX turtleconservationorganizations_created_at_idx ON turtleconservationorganizations(created_at DESC);
CREATE INDEX turtleconservationorganizations_name_idx ON turtleconservationorganizations(name);


-- RLS Policies (Granular)
ALTER TABLE turtleconservationorganizations ENABLE ROW LEVEL SECURITY;

-- SELECT: Users can only view their own records
CREATE POLICY "turtleconservationorganizations_select_own"
  ON turtleconservationorganizations FOR SELECT
  USING (auth.uid() = user_id);

-- INSERT: Users can only create records for themselves
CREATE POLICY "turtleconservationorganizations_insert_own"
  ON turtleconservationorganizations FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- UPDATE: Users can only update their own records
CREATE POLICY "turtleconservationorganizations_update_own"
  ON turtleconservationorganizations FOR UPDATE
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);

-- DELETE: Users can only delete their own records
CREATE POLICY "turtleconservationorganizations_delete_own"
  ON turtleconservationorganizations FOR DELETE
  USING (auth.uid() = user_id);
```

Relationships:
- many_to_many to TurtleSpecies (foreign key: turtlespecies_id)


### 3. Updated-At Automation

Create a **Trigger Function** that automatically sets `updated_at`:
- Function Name: `update_updated_at()`
- Trigger: BEFORE UPDATE on all tables
- Logic: SET NEW.updated_at = now()

Apply the trigger to all tables (profiles + entities).

### 4. Storage Setup

Create a **Storage Bucket** for file uploads:
- Name: "uploads"
- Visibility: Private
- Folder Structure: `{user_id}/{filename}`

Storage Policies:
- Users can only upload to their own folder
- Users can only read/delete their own files
- No public access

Implement folder-based security with storage.foldername().

### 5. Type Safety

Generate TypeScript types from the schema:
- Use Supabase CLI: `supabase gen types typescript`
- Export to: `lib/database.types.ts`
- Use the types for type-safe queries

---

## Validation Checklist

Verify that:
- [ ] profiles table exists with RLS
- [ ] Auto-create Profile Trigger works
- [ ] TurtleSpecies table exists with correct fields
- [ ] TurtleConservationOrganization table exists with correct fields
- [ ] All tables have RLS enabled
- [ ] Foreign Keys correctly set
- [ ] Indexes created for performance
- [ ] updated_at Trigger on all tables
- [ ] Storage Bucket configured
- [ ] TypeScript Types generated

**Test:**
- Signup new user → Profile automatically created
- Create Entity record → Only visible to owner
- Update record → updated_at automatically set
- Upload file → Only possible in own folder

---

## Troubleshooting

**RLS Issues:**
- Check if policies correctly use auth.uid()
- Test with SQL Editor: `SELECT auth.uid()`
- Policies must have USING and WITH CHECK

**Foreign Key Errors:**
- Check CASCADE on user_id
- Entities with relationships: Correct order when creating

**Storage Issues:**
- Folder structure must follow {user_id}/ pattern
- Policies use storage.foldername() for user isolation

---

**Continue to:** `03-auth.md`
