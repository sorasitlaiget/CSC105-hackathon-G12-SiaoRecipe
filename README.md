# SiaoRecipe – Satirical Culinary Social Platform

A full-stack satirical social web application built for the Freshman Hackathon (*"Silly but Useful"* theme), enabling emotion-driven recipe sharing, dynamic community feeds, and structured relational data management. Built with **React**, **Node.js**, **Prisma ORM**, and **SQLite**.

---

## Overview

SiaoRecipe bridges comedic storytelling with functional web architecture. Instead of conventional culinary apps, SiaoRecipe allows users to vent, express relatable emotional states, and share comfort food recipes tagged by sentiment. Behind the playful UI lies a strictly modeled relational database enforcing data integrity, cascade operations, and duplicate prevention.

---

## Key Features

* **Dynamic Recipe Feed & Post Creation:** Interactive feed with real-time UI updates, structured recipe steps, and author attribution.
* **Emotion-Driven Tagging:** Recipes are indexed by sentiment taxonomies using structured database enums (e.g., Heartbroken, Broke, Nostalgic).
* **Interactive Community Threads:** Nested recipe discussions with real-time comment additions and author tracking.
* **Guaranteed Unique Engagements:** Database-enforced single-like constraint preventing duplicate reactions per user per recipe.
* **Strict Referential Integrity:** Automated cascading deletions ensuring clean database state when users or posts are removed.

---

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Frontend** | React.js, JavaScript, CSS3 / Tailwind CSS |
| **Backend** | Node.js, Express.js |
| **ORM** | Prisma ORM |
| **Database** | SQLite (Relational) |
| **API Architecture** | RESTful APIs (JSON payloads) |

---

## Database Schema & Engineering Highlights

### 1. Composite Unique Constraint (`Like` Engine)
To prevent duplicate reactions and eliminate frontend race conditions, the database schema enforces a composite unique constraint across `userId` and `recipeId`.

```prisma
model Like {
  id        String   @id @default(uuid())
  userId    String
  recipeId  String
  createdAt DateTime @default(now())

  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  recipe    Recipe   @relation(fields: [recipeId], references: [id], onDelete: Cascade)

  @@unique([userId, recipeId])
}
