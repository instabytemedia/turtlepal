# 06 - Entity CRUD

> **Agent:** Entities Agent 📦
> **Goal:** Create complete CRUD for all Entities

---

## Context

**Product:** TurtlePal
**Entities:** TurtleSpecies, TurtleConservationOrganization
**Entity Count:** 2

---

## Overview

Per Entity will be created:
- 1x TypeScript Types File
- 1x Zod Schema File
- 2x API Routes (List/Create + Detail)
- 1x SWR Hooks File
- 2x Pages (List + Detail)
- 2x Components (List + Detail)

**Total Files per Entity:** 9
**Total Files overall:** 18

| Entity | Table | API Endpoints | Pages |
|--------|-------|---------------|-------|
| TurtleSpecies | turtlespeciess | /api/turtlespeciess | /turtlespeciess, /turtlespeciess/[id] |
| TurtleConservationOrganization | turtleconservationorganizations | /api/turtleconservationorganizations | /turtleconservationorganizations, /turtleconservationorganizations/[id] |

---


---

## Entity 1: TurtleSpecies

**Description:** A turtle species entity
**Table Name:** `turtlespeciess`

**Fields:**
- `id`: uuid - Primary key
- `created_at`: datetime - Creation timestamp
- `updated_at`: datetime - Last update timestamp
- `user_id`: uuid - Owner user ID
- `name`: string - Name of the turtle species
- `description`: text (optional) - Description of the turtle species

**Relationships:**
- many_to_many → TurtleConservationOrganization: A turtle species can be associated with multiple conservation organizations





### Instructions: TurtleSpecies

#### 1. TypeScript Types

Create **Types File** (`types/turtlespecies.ts`):

**⚠️ CRITICAL: No Duplicate Fields!**
Each field must appear EXACTLY ONCE. Use consistent naming (snake_case for DB fields).

```typescript
// ✅ CORRECT: Each field once
export interface TurtleSpecies {
  id: string;
  user_id: string;
  created_at: string;
  updated_at: string;
  // Custom fields:
  name: string;
  description: string;
}

// ❌ WRONG: Duplicate fields!
export interface TurtleSpecies {
  id: string;        // 1x
  userId: string;    // mixed camelCase
  created_at: Date;  // wrong type (use string)
  user_id: string;   // DUPLICATE (snake_case)!
  id: string;        // DUPLICATE!
}
```

**TurtleSpeciesListItem Interface:**
- Reduced version for List Views
- Only essential fields: id, created_at + first 3 custom fields

#### 2. Zod Validation Schema

Create **Schema File** (`lib/schemas/turtlespecies.ts`):

**CreateTurtleSpeciesSchema:**
- Zod Object with all custom fields
- Required vs Optional based on Field Definition
- Type Mapping:
  - string/text → z.string()
  - number/integer → z.number() / z.number().int()
  - boolean → z.boolean()
  - date/datetime → z.string() (ISO format)

**UpdateTurtleSpeciesSchema:**
- Use Create Schema with .partial()
- All fields optional for updates

**Exports:**
- Schemas + Inferred Types

#### 3. API Routes - List & Create

Create **List/Create Route** (`app/api/turtlespeciess/route.ts`):

**GET /api/turtlespeciess (List):**
- Auth Check: User from Supabase
- Query Params: limit (default 20), cursor (pagination)
- Supabase Query:
  - Filter: user_id = current user
  - Order: created_at DESC
  - Limit: limit + 1 (for hasMore detection)
  - Cursor: created_at < cursor (if provided)
- Response: { data: items, meta: { next_cursor, has_more } }
- Error Handling: 401 (unauthorized), 500 (server error)

**POST /api/turtlespeciess (Create):**
- Auth Check: User from Supabase
- Body Validation: CreateTurtleSpeciesSchema with safeParse
- Insert: Merge validated data + user_id
- Select: .select().single() for Created Object
- Response: { data } with 201 Status
- Error Handling: 400 (validation), 500 (server error)

#### 4. API Routes - Detail Operations

Create **Detail Route** (`app/api/turtlespeciess/[id]/route.ts`):

**GET /api/turtlespeciess/[id] (Read):**
- Params: id (await params promise)
- Auth Check: User from Supabase
- Query: .select().eq("id", id).eq("user_id", user.id).single()
- Response: { data }
- Error: 404 if not found

**PATCH /api/turtlespeciess/[id] (Update):**
- Params: id (await params promise)
- Auth Check + Body Validation
- Update: .update().eq("id").eq("user_id").select().single()
- Response: { data }
- Error: 400 (validation), 404 (not found)

**DELETE /api/turtlespeciess/[id] (Delete):**
- Params: id (await params promise)
- Auth Check
- Delete: .delete().eq("id").eq("user_id")
- Response: 204 No Content
- Error: 404 (not found)

**Important for all Routes:**
- User ID Check prevents access to other users' data
- NextRequest/NextResponse for Types
- await params as Promise (Next.js 15)

#### 5. SWR Data Fetching Hooks

Create **Hooks File** (`hooks/useTurtleSpeciess.ts`):

**useTurtleSpeciess() Hook:**
- SWR for List Fetching
- URL: /api/turtlespeciess
- Returns: { turtlespeciess, isLoading, error, mutate }

**useTurtleSpecies(id) Hook:**
- SWR for Single Item Fetching
- URL: /api/turtlespeciess/{id}
- Conditional: only fetch if id is provided
- Returns: { turtlespecies, isLoading, error, mutate }

**useCreateTurtleSpecies() Hook:**
- POST Request Function
- Mutate List after Success
- Error Handling with throw

**useUpdateTurtleSpecies(id) Hook:**
- PATCH Request Function
- Mutate Item + List after Success
- Error Handling with throw

**useDeleteTurtleSpecies() Hook:**
- DELETE Request Function
- Mutate List after Success
- Error Handling with throw

**Important:**
- All Hooks are Client Components ("use client")
- Fetcher Function for SWR
- Proper Error Handling

#### 6. List Page

Create **List Page** (`app/(app)/turtlespeciess/page.tsx`):

**Functionality:**
- Server Component for Auth Check
- Redirect to /login if not authenticated
- Client Component for List Rendering

**Layout:**
- Container with Padding
- Header: Title + "New" Button (right)
- List Component Integration

#### 7. List Component

Create **List Component** (`components/turtlespecies/TurtleSpeciesList.tsx`):

**States:**
- Loading: Skeleton Grid (6 items)
- Error: Error Message Card
- Empty: EmptyState Component with "Create New" Action
- Success: Grid with Items

**Grid Layout:**
- Responsive: 1 col (mobile), 2 col (tablet), 3 col (desktop)
- Gap: gap-4

**Item Cards:**
- Link to Detail Page
- Hover Effect: border-primary
- Content: Title + Created Date (adapt to Entity fields)

**Important:**
- "use client" Directive
- useTurtleSpeciess() Hook for Data
- Proper Loading/Error/Empty States

#### 8. Detail Page

Create **Detail Page** (`app/(app)/turtlespeciess/[id]/page.tsx`):

**Functionality:**
- Server Component
- Auth Check
- Direct Supabase Fetch for SSR
- notFound() if Item doesn't exist
- Pass Data to Detail Component

**Security:**
- user_id Check in Query

#### 9. Detail Component

Create **Detail Component** (`components/turtlespecies/TurtleSpeciesDetail.tsx`):

**Props:**
- `turtlespecies`: TurtleSpecies Object

**Layout:**
- Back Button (top left)
- Delete Button (top right)
- Card with Details:
  - ID
  - All Custom Fields (from Entity Definition)
  - Created/Updated Timestamps

**Delete Functionality:**
- Confirm Dialog
- useDeleteTurtleSpecies() Hook
- Router Push to List after Success
- Loading State during Delete

**Important:**
- "use client" Directive
- Router for Navigation
- Confirmation before Delete



---

## Entity 2: TurtleConservationOrganization

**Description:** A turtle conservation organization entity
**Table Name:** `turtleconservationorganizations`

**Fields:**
- `id`: uuid - Primary key
- `created_at`: datetime - Creation timestamp
- `updated_at`: datetime - Last update timestamp
- `user_id`: uuid - Owner user ID
- `name`: string - Name of the conservation organization
- `description`: text (optional) - Description of the conservation organization

**Relationships:**
- many_to_many → TurtleSpecies: A conservation organization can be associated with multiple turtle species





### Instructions: TurtleConservationOrganization

#### 1. TypeScript Types

Create **Types File** (`types/turtleconservationorganization.ts`):

**⚠️ CRITICAL: No Duplicate Fields!**
Each field must appear EXACTLY ONCE. Use consistent naming (snake_case for DB fields).

```typescript
// ✅ CORRECT: Each field once
export interface TurtleConservationOrganization {
  id: string;
  user_id: string;
  created_at: string;
  updated_at: string;
  // Custom fields:
  name: string;
  description: string;
}

// ❌ WRONG: Duplicate fields!
export interface TurtleConservationOrganization {
  id: string;        // 1x
  userId: string;    // mixed camelCase
  created_at: Date;  // wrong type (use string)
  user_id: string;   // DUPLICATE (snake_case)!
  id: string;        // DUPLICATE!
}
```

**TurtleConservationOrganizationListItem Interface:**
- Reduced version for List Views
- Only essential fields: id, created_at + first 3 custom fields

#### 2. Zod Validation Schema

Create **Schema File** (`lib/schemas/turtleconservationorganization.ts`):

**CreateTurtleConservationOrganizationSchema:**
- Zod Object with all custom fields
- Required vs Optional based on Field Definition
- Type Mapping:
  - string/text → z.string()
  - number/integer → z.number() / z.number().int()
  - boolean → z.boolean()
  - date/datetime → z.string() (ISO format)

**UpdateTurtleConservationOrganizationSchema:**
- Use Create Schema with .partial()
- All fields optional for updates

**Exports:**
- Schemas + Inferred Types

#### 3. API Routes - List & Create

Create **List/Create Route** (`app/api/turtleconservationorganizations/route.ts`):

**GET /api/turtleconservationorganizations (List):**
- Auth Check: User from Supabase
- Query Params: limit (default 20), cursor (pagination)
- Supabase Query:
  - Filter: user_id = current user
  - Order: created_at DESC
  - Limit: limit + 1 (for hasMore detection)
  - Cursor: created_at < cursor (if provided)
- Response: { data: items, meta: { next_cursor, has_more } }
- Error Handling: 401 (unauthorized), 500 (server error)

**POST /api/turtleconservationorganizations (Create):**
- Auth Check: User from Supabase
- Body Validation: CreateTurtleConservationOrganizationSchema with safeParse
- Insert: Merge validated data + user_id
- Select: .select().single() for Created Object
- Response: { data } with 201 Status
- Error Handling: 400 (validation), 500 (server error)

#### 4. API Routes - Detail Operations

Create **Detail Route** (`app/api/turtleconservationorganizations/[id]/route.ts`):

**GET /api/turtleconservationorganizations/[id] (Read):**
- Params: id (await params promise)
- Auth Check: User from Supabase
- Query: .select().eq("id", id).eq("user_id", user.id).single()
- Response: { data }
- Error: 404 if not found

**PATCH /api/turtleconservationorganizations/[id] (Update):**
- Params: id (await params promise)
- Auth Check + Body Validation
- Update: .update().eq("id").eq("user_id").select().single()
- Response: { data }
- Error: 400 (validation), 404 (not found)

**DELETE /api/turtleconservationorganizations/[id] (Delete):**
- Params: id (await params promise)
- Auth Check
- Delete: .delete().eq("id").eq("user_id")
- Response: 204 No Content
- Error: 404 (not found)

**Important for all Routes:**
- User ID Check prevents access to other users' data
- NextRequest/NextResponse for Types
- await params as Promise (Next.js 15)

#### 5. SWR Data Fetching Hooks

Create **Hooks File** (`hooks/useTurtleConservationOrganizations.ts`):

**useTurtleConservationOrganizations() Hook:**
- SWR for List Fetching
- URL: /api/turtleconservationorganizations
- Returns: { turtleconservationorganizations, isLoading, error, mutate }

**useTurtleConservationOrganization(id) Hook:**
- SWR for Single Item Fetching
- URL: /api/turtleconservationorganizations/{id}
- Conditional: only fetch if id is provided
- Returns: { turtleconservationorganization, isLoading, error, mutate }

**useCreateTurtleConservationOrganization() Hook:**
- POST Request Function
- Mutate List after Success
- Error Handling with throw

**useUpdateTurtleConservationOrganization(id) Hook:**
- PATCH Request Function
- Mutate Item + List after Success
- Error Handling with throw

**useDeleteTurtleConservationOrganization() Hook:**
- DELETE Request Function
- Mutate List after Success
- Error Handling with throw

**Important:**
- All Hooks are Client Components ("use client")
- Fetcher Function for SWR
- Proper Error Handling

#### 6. List Page

Create **List Page** (`app/(app)/turtleconservationorganizations/page.tsx`):

**Functionality:**
- Server Component for Auth Check
- Redirect to /login if not authenticated
- Client Component for List Rendering

**Layout:**
- Container with Padding
- Header: Title + "New" Button (right)
- List Component Integration

#### 7. List Component

Create **List Component** (`components/turtleconservationorganization/TurtleConservationOrganizationList.tsx`):

**States:**
- Loading: Skeleton Grid (6 items)
- Error: Error Message Card
- Empty: EmptyState Component with "Create New" Action
- Success: Grid with Items

**Grid Layout:**
- Responsive: 1 col (mobile), 2 col (tablet), 3 col (desktop)
- Gap: gap-4

**Item Cards:**
- Link to Detail Page
- Hover Effect: border-primary
- Content: Title + Created Date (adapt to Entity fields)

**Important:**
- "use client" Directive
- useTurtleConservationOrganizations() Hook for Data
- Proper Loading/Error/Empty States

#### 8. Detail Page

Create **Detail Page** (`app/(app)/turtleconservationorganizations/[id]/page.tsx`):

**Functionality:**
- Server Component
- Auth Check
- Direct Supabase Fetch for SSR
- notFound() if Item doesn't exist
- Pass Data to Detail Component

**Security:**
- user_id Check in Query

#### 9. Detail Component

Create **Detail Component** (`components/turtleconservationorganization/TurtleConservationOrganizationDetail.tsx`):

**Props:**
- `turtleconservationorganization`: TurtleConservationOrganization Object

**Layout:**
- Back Button (top left)
- Delete Button (top right)
- Card with Details:
  - ID
  - All Custom Fields (from Entity Definition)
  - Created/Updated Timestamps

**Delete Functionality:**
- Confirm Dialog
- useDeleteTurtleConservationOrganization() Hook
- Router Push to List after Success
- Loading State during Delete

**Important:**
- "use client" Directive
- Router for Navigation
- Confirmation before Delete


---

## Validation Checklist

Verify for EACH Entity:
- [ ] Types File created with correct Interfaces
- [ ] Zod Schema File with Create + Update Schemas
- [ ] API List/Create Route works
- [ ] API Detail Route works (GET, PATCH, DELETE)
- [ ] SWR Hooks File with all 5 Hooks
- [ ] List Page with Auth Check
- [ ] List Component with Loading/Error/Empty States
- [ ] Detail Page with Auth Check
- [ ] Detail Component with Delete Functionality

**Test per Entity:**
1. **Create:** Go to /turtlespeciess/new → Create Item → Redirect to List
2. **List:** List shows new Item
3. **Read:** Click on Item → Detail Page loads
4. **Update:** (built in Phase 08-forms)
5. **Delete:** Delete Button → Confirmation → Back to List

**API Tests:**
- POST /api/[entity]s → 201 with Created Item
- GET /api/[entity]s → 200 with Array
- GET /api/[entity]s/[id] → 200 with Object
- PATCH /api/[entity]s/[id] → 200 with Updated Item
- DELETE /api/[entity]s/[id] → 204

---

## Troubleshooting

**API 401 Errors:**
- Check Auth Check in all Routes
- Supabase Client correctly created?
- User is correctly retrieved?

**RLS Policy Errors:**
- Check if user_id is in Query
- RLS Policies active in Supabase?

**Pagination not working:**
- Cursor Logic: created_at < cursor
- hasMore Detection: length > limit

**SWR not updating:**
- Call mutate() after Mutations
- Fetcher Function correct?

**TypeScript Errors:**
- Types from database.types.ts generated?
- Zod Schemas export inferred Types

---

**Continue to:** `07-dashboard.md`
