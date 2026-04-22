# FitCheck — AI Daily Workout Planner

A mobile-first web app that generates a personalized daily workout plan based on how you feel each morning.

**Course project · Columbia University · Spring 2026**  
**Built with:** HTML · CSS · JavaScript · Claude API

---

## The Problem

Every morning I wake up and have no idea what to work out. I'll spend 10 minutes just scrolling my phone. FitCheck fixes that.

## How It Works

1. **Profile setup (once)** — enter your age, fitness goals, available equipment, and the YouTube creators you like to follow
2. **Morning check-in (30 seconds)** — log your sleep quality, energy level, soreness, and stress
3. **Get your plan** — two versions are generated simultaneously:
   - **Follow-along** — matches a creator video to your energy level, fills remaining time with targeted exercises
   - **Workout plan** — a complete independent workout with sets, reps, and coaching cues

The app saves your profile locally, so returning users skip straight to the daily check-in.

## Features

- Cycle phase tracking for female users — automatically adjusts workout intensity
- 14+ fitness creators (Elenfit, Chloe Ting, Caroline Girvan, Yoga with Adriene, and more)
- Time-aware planning — video duration is factored into total session time
- Works as an installable app on iPhone and Android (PWA)
- Built with Claude as a coding partner — from UI design to API integration

## Files

| File | Purpose |
|------|---------|
| `index.html` | Entire app — single file, no build step needed |
| `manifest.json` | PWA config — enables "Add to Home Screen" |
| `sw.js` | Service worker — offline support |
| `icon-192.png` / `icon-512.png` | Home screen icons |

