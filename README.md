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
2. Referential Integrity & Cascade Deletes
Configured onDelete: Cascade across all relational foreign keys (User -> Recipes, Recipe -> Comments, Recipe -> Likes).
Deleting a recipe or user automatically purges orphaned comments and reactions without orphaned database records.
3. Enum Taxonomies for Sentiments
Replaced arbitrary string tags with fixed database enum taxonomies to keep search filtering fast and consistent across endpoints.
Project Structure
Plaintext
siaorecipe/
├── client/                 # React Frontend
│   ├── src/
│   │   ├── components/     # RecipeCard, CommentSection, EmotionFilter
│   │   ├── pages/          # Feed, CreateRecipe, RecipeDetail
│   │   └── services/       # Axios / Fetch API client
├── server/                 # Node.js REST API
│   ├── prisma/
│   │   └── schema.prisma   # Data models, constraints, and enums
│   ├── routes/             # Express API endpoints (/recipes, /likes, /comments)
│   ├── controllers/        # Business logic & Prisma query executions
│   └── index.js            # Server entry point
└── package.json
Getting Started
1. Clone the repository
Bash
git clone [https://github.com/sorasitlaiget/siaorecipe.git](https://github.com/sorasitlaiget/siaorecipe.git)
cd siaorecipe
2. Backend Setup
Bash
cd server
npm install
npx prisma migrate dev --name init
npm run dev
3. Frontend Setup
Bash
cd ../client
npm install
npm start
