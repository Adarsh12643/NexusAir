# NexusAir - Flight Management System

A production-like Flight Management web application built with Next.js/React, Tailwind CSS, and Zustand. The application features a fully responsive booking flow, live seat selection, and a premium "Immersive UI" dark theme aesthetic.

## Features

- **Immersive UI**: A bold, dark aesthetic with dynamic gradients, glassmorphism, and a sci-fi/terminal-inspired vibe.
- **Flight Search**: Search for flights by origin, destination, date, and passenger count.
- **Interactive Seat Map**: Live visual representation of the aircraft cabin with interactive selection.
- **Booking Flow**: Complete multi-step booking process with passenger details and PNR generation.
- **Booking Management**: View, reschedule, or cancel bookings in a dedicated dashboard.

## Tech Stack

- **Frontend**: React 19 + TypeScript + Vite
- **Styling**: Tailwind CSS v4, Lucide React icons
- **State Management**: Zustand with `persist` middleware
- **Routing**: React Router DOM

## Running Locally

1. Install dependencies:
   ```bash
   npm install
   ```

2. Setup Environment Variables:
   Copy `.env.example` to `.env` and fill in your Supabase credentials:
   ```bash
   VITE_SUPABASE_URL=your_supabase_project_url
   VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
   ```
   *(Note: The current implementation runs using provided local mock data if Supabase keys are absent for easy previewing)*.

3. Start the development server:
   ```bash
   npm run dev
   ```

## Supabase Configuration & Schema

If connecting to a live Supabase instance, execute the provided SQL migration files found in `/supabase/migrations`.
This sets up:
- Relational tables: `flights`, `seats`, `bookings`, `passengers`, `reschedules`.
- **Row Level Security (RLS)**: Users can only query and mutate their own bookings.
- **Functions & Triggers**:
  - `reserve_seat`: An RPC transaction that avoids double-booking.
  - `cancel_booking`: RPC to free an occupied seat and update status.
  - Database trigger to block cancellations within 2 hours of departure.

## Zustand Store Structure

The application's state is decoupled into specific stores designed for persistence and security:

- **`useFlightStore`**: Manages the transient lifecycle of a booking.
  - **Stored Data**: Search query, selected flight/seat, active step, and passenger form data.
  - **Persistence (`persist` middleware)**: Allows users to refresh or restore their tab and resume their booking seamlessly.
  - **Security (`partialize`)**: Explicitly strips out sensitive data like the passport number before committing state to `localStorage`.

- **`useUserStore`**: Manages authentication sessions (caching mocked session data for the demo).
