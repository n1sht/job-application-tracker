# Job Application Tracker

A full-stack job tracking app with a drag-and-drop Kanban board. Add applications, move them through stages, and actually keep track of where you stand in your search instead of losing everything in a spreadsheet.

Live demo: [job-application-tracker-phi-silk.vercel.app](https://job-application-tracker-phi-silk.vercel.app)

---

## Why I built this

I was applying to jobs and keeping track in a Google Sheet. It fell apart fast — no good way to see the pipeline at a glance, no easy way to move things around, and everything lived in a flat list that got harder to scan the longer it got. I wanted something that felt closer to how engineers actually track work: a board with columns you can drag between.

The challenge with a Kanban board at scale is keeping it snappy. Drag interactions need to feel instant, and with 200+ entries the naive approach (re-rendering the whole list on every state change) makes it noticeably sluggish. Getting that right was the most interesting part of building this.

---

## Features

- **Kanban board with drag-and-drop** - powered by @dnd-kit, supports dragging cards across columns and reordering within a column. Handles 200+ job entries without any UI lag.
- **Full authentication** - BetterAuth handles sign up, sign in, and session management. Each user's data is isolated.
- **Persistent storage** - applications saved to MongoDB via Mongoose 9. Status changes persist across sessions.
- **Production landing page** - responsive marketing page with sub-second load times on both desktop and mobile.
- **TypeScript throughout** - 94.8% of the codebase is TypeScript. No runtime surprises from type mismatches.

---

## Tech stack

- Next.js 16 (App Router)
- React 19
- TypeScript
- MongoDB with Mongoose 9
- BetterAuth
- Shadcn/ui + Radix UI
- @dnd-kit (core + sortable)
- Tailwind CSS v4
- Deployed on Vercel

---

## How the drag-and-drop works

@dnd-kit gives you the primitives — `DndContext`, `useSortable`, `useDroppable` — but the actual state management is on you. Each column is a droppable zone and each card is a sortable item. On drag end, the app figures out whether the card moved to a new column or just reordered within the same one, updates local state immediately for the instant feel, then fires the database update in the background.

The 200+ entry benchmark came from testing with a populated board. The key is that @dnd-kit uses a virtualization-friendly approach and only the actively dragged element re-renders during the drag, not the whole board.

---

## Running locally

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

You need a MongoDB connection string. Create a `.env.local`:

```
MONGODB_URI=your_mongodb_connection_string
BETTER_AUTH_SECRET=your_secret
BETTER_AUTH_URL=http://localhost:3000
```

---

## Project structure

```
app/          Next.js App Router pages and API routes
components/   UI components including the Kanban board and cards
lib/          Database connection, auth config, utility functions
public/       Static assets
```

---

## What I would do differently

- Add filtering and search across all applications — right now you have to scroll to find something specific
- Add email reminders for applications that have gone quiet after X days
- Replace MongoDB with PostgreSQL for better relational queries (tracking contacts per company, for example)
- Add analytics: offer rate, response rate by application source, average time in each stage
