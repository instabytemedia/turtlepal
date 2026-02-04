# 08 - Forms & Validation

> **Agent:** Forms Agent 📝
> **Goal:** Create forms with react-hook-form + Zod

---

## Context

**Product:** TurtlePal
**Entities with Forms:** TurtleSpecies, TurtleConservationOrganization

---

## Instructions

### 1. Check Dependencies

Ensure the following dependencies are installed:
- `react-hook-form` - Form State Management
- `@hookform/resolvers` - Zod Integration

If not present, install them.

**Important:**
- Zod should already be installed from Setup
- Schemas should already exist in lib/schemas/ (from Phase 06)


### 2. TurtleSpecies Form

Create **TurtleSpeciesForm Component** (`components/turtlespecies/TurtleSpeciesForm.tsx`):

**Props Interface:**
- `turtlespecies?`: TurtleSpecies Object (if provided = Edit Mode)
- `onSuccess?`: Callback after successful submit

**Functionality:**
- Client Component ("use client")
- react-hook-form with zodResolver
- Use CreateTurtleSpeciesSchema for Validation
- Distinguish Create vs Edit Mode (based on turtlespecies prop)
- useCreateTurtleSpecies() Hook for Create
- useUpdateTurtleSpecies(id) Hook for Edit
- Loading State during Submit
- Error Handling with Alert
- After Success: onSuccess Callback + Router Push to List + Router Refresh

**Form Fields:**
- `id` (text) - Required: Primary key
- `created_at` (text) - Required: Creation timestamp
- `updated_at` (text) - Required: Last update timestamp
- `user_id` (text) - Required: Owner user ID
- `name` (text) - Required: Name of the turtle species
- `description` (text) - Optional: Description of the turtle species

**Layout:**
- Card Container
- CardHeader: Title ("New TurtleSpecies" or "Edit TurtleSpecies")
- CardContent: Form Fields (space-y-4)
- CardFooter: Actions (justify-end gap-2)
  - Cancel Button (variant="outline", onClick → router.back())
  - Submit Button (type="submit", disabled during Loading)

**Field Layout (per field):**
- Container with space-y-2
- Label with htmlFor
- Input with register()
- Error Message (if errors.fieldName)

**Important:**
- register() with valueAsNumber for Number fields
- defaultValues from turtlespecies prop (for Edit Mode)
- Proper Error Display under each field

---

Create **New TurtleSpecies Page** (`app/(app)/turtlespeciess/new/page.tsx`):

**Functionality:**
- Server Component for Auth Check
- Redirect to /login if not authenticated
- Render TurtleSpeciesForm Component

**Layout:**
- Container with max-w-2xl + py-8
- Centered Form Layout



### 3. TurtleConservationOrganization Form

Create **TurtleConservationOrganizationForm Component** (`components/turtleconservationorganization/TurtleConservationOrganizationForm.tsx`):

**Props Interface:**
- `turtleconservationorganization?`: TurtleConservationOrganization Object (if provided = Edit Mode)
- `onSuccess?`: Callback after successful submit

**Functionality:**
- Client Component ("use client")
- react-hook-form with zodResolver
- Use CreateTurtleConservationOrganizationSchema for Validation
- Distinguish Create vs Edit Mode (based on turtleconservationorganization prop)
- useCreateTurtleConservationOrganization() Hook for Create
- useUpdateTurtleConservationOrganization(id) Hook for Edit
- Loading State during Submit
- Error Handling with Alert
- After Success: onSuccess Callback + Router Push to List + Router Refresh

**Form Fields:**
- `id` (text) - Required: Primary key
- `created_at` (text) - Required: Creation timestamp
- `updated_at` (text) - Required: Last update timestamp
- `user_id` (text) - Required: Owner user ID
- `name` (text) - Required: Name of the conservation organization
- `description` (text) - Optional: Description of the conservation organization

**Layout:**
- Card Container
- CardHeader: Title ("New TurtleConservationOrganization" or "Edit TurtleConservationOrganization")
- CardContent: Form Fields (space-y-4)
- CardFooter: Actions (justify-end gap-2)
  - Cancel Button (variant="outline", onClick → router.back())
  - Submit Button (type="submit", disabled during Loading)

**Field Layout (per field):**
- Container with space-y-2
- Label with htmlFor
- Input with register()
- Error Message (if errors.fieldName)

**Important:**
- register() with valueAsNumber for Number fields
- defaultValues from turtleconservationorganization prop (for Edit Mode)
- Proper Error Display under each field

---

Create **New TurtleConservationOrganization Page** (`app/(app)/turtleconservationorganizations/new/page.tsx`):

**Functionality:**
- Server Component for Auth Check
- Redirect to /login if not authenticated
- Render TurtleConservationOrganizationForm Component

**Layout:**
- Container with max-w-2xl + py-8
- Centered Form Layout


---

## Validation Checklist

Verify for EACH Entity:
- [ ] Form Component created
- [ ] New Page created with Auth Check
- [ ] react-hook-form integrated
- [ ] Zod Validation works
- [ ] Create Mode works
- [ ] Edit Mode works (when turtlespecies is passed)
- [ ] Loading State during Submit
- [ ] Error Messages are displayed
- [ ] Redirect after Success

**Test per Entity:**
1. **Create Test:**
   - Go to /[entity]s/new
   - Leave field empty → Validation Error displayed
   - Fill all fields → Submit
   - Redirected to list
   - New entry appears in list

2. **Validation Test:**
   - Required fields empty → Error
   - Number field with text → Error (if applicable)
   - Correct input → No Error

3. **Edit Test (if Detail Page exists):**
   - Open Entity Detail
   - Click Edit
   - Fields are pre-filled
   - Change value → Submit
   - Changes saved

---

## Troubleshooting

**Form submitted but nothing happens:**
- handleSubmit correctly passed to form onSubmit?
- async/await in onSubmit Handler?
- Errors being logged?

**Validation not working:**
- zodResolver correctly imported?
- Schema correctly passed to zodResolver?
- Schema exported from lib/schemas?

**Register not working:**
- Field Name exactly like in Schema?
- Input has {...register("fieldName")} spread?
- For Number: valueAsNumber Option?

**defaultValues being ignored:**
- useForm with defaultValues option?
- turtlespecies prop being passed?
- Object spread correct?

**No redirect after Submit:**
- router.push() called?
- router.refresh() for SSR Update?
- No Error in try/catch?

**TypeScript Errors:**
- CreateTurtleSpeciesSchema imported?
- Type Inference from Zod: type CreateTurtleSpecies = z.infer<typeof CreateTurtleSpeciesSchema>
- useForm Generic: useForm<CreateTurtleSpecies>()

---

**Continue to:** `09-landing.md`
