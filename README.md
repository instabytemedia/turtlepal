# TurtlePal

> Care for turtles, virtually

This variant focuses on creating a virtual pet experience where users can care for and learn about turtles. It's designed for kids and educational institutions, promoting conservation and responsibility. The app includes interactive games, quizzes, and a virtual habitat for the turtles.

## Features

- Virtual Pet
- Educational Content
- Interactive Games

## Tech Stack

- **Framework:** Next.js 14 (App Router)
- **Database:** Supabase (PostgreSQL)
- **Auth:** Supabase Auth
- **Styling:** Tailwind CSS
- **Language:** TypeScript

## Getting Started

1. Clone this repository
2. Copy `.env.example` to `.env.local` and fill in your credentials
3. Run `npm install`
4. Run `npm run dev`

## Project Structure

```
├── app/                  # Next.js App Router pages
├── components/           # React components
├── lib/                  # Utilities and helpers
├── supabase/            # Database schema
└── INSTRUCTIONS.md      # Detailed build guide for AI assistants
```

## Database

This project uses 2 main entities:
- **TurtleSpecies**: A turtle species entity
- **TurtleConservationOrganization**: A turtle conservation organization entity

## Build Instructions

For detailed step-by-step build instructions, see [`INSTRUCTIONS.md`](./INSTRUCTIONS.md).

This file contains comprehensive guidance for building this project with AI coding assistants like Claude Code, Cursor, or Windsurf.

---

*Generated with [Claudery](https://claudery.io) - AI-powered blueprint generator*
