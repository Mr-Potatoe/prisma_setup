# Prisma Setup Guide

## Introduction

Prisma is a modern database toolkit that helps you manage your database schema and interact with your database using a type-safe ORM. This guide will walk you through setting up Prisma, defining your schema, running migrations, and using Prisma Client to query your database.

## Prerequisites

- Node.js (>=12.0.0)
- npm or Yarn

## Installation

1. **Initialize a New Project**

   If you don't have a Node.js project set up yet, create one:

   ```bash
   mkdir my-prisma-project
   cd my-prisma-project
   npm init -y
   ```

2. **Install Prisma**

   Install Prisma CLI and Prisma Client:

   ```bash
   npm install @prisma/client
   npm install prisma --save-dev
   ```

3. **Initialize Prisma**

   Generate the Prisma setup files:

   ```bash
   npx prisma init
   ```

   This command creates a `prisma` directory with a `schema.prisma` file and a `.env` file.

## Configuring the Database

1. **Set Up Your Database URL**

   Open the `.env` file and configure the `DATABASE_URL` environment variable with your database connection string. For example:

   ```env
   DATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/mydatabase"
   ```

2. **Define Your Schema**

   Edit the `prisma/schema.prisma` file to define your data models. For example:

   ```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }

   generator client {
     provider = "prisma-client-js"
   }

   model User {
     id    Int    @id @default(autoincrement())
     name  String
     email String @unique
   }

   model Post {
     id      Int    @id @default(autoincrement())
     title   String
     content String
     author  User   @relation(fields: [authorId], references: [id])
     authorId Int
   }
   ```

## Running Migrations

1. **Create a Migration**

   Generate a migration from your schema changes:

   ```bash
   npx prisma migrate dev --name init
   ```

   This command creates a migration file and applies it to your database.

2. **View Migration Status**

   Check the status of your migrations:

   ```bash
   npx prisma migrate status
   ```

## Using Prisma Client

1. **Generate Prisma Client**

   Generate Prisma Client to interact with your database:

   ```bash
   npx prisma generate
   ```

2. **Use Prisma Client in Your Application**

   In your JavaScript or TypeScript code, you can use Prisma Client to query your database. Here’s an example using Node.js:

   ```javascript
   // index.js
   const { PrismaClient } = require('@prisma/client');
   const prisma = new PrismaClient();

   async function main() {
     // Create a new user
     const newUser = await prisma.user.create({
       data: {
         name: 'Alice',
         email: 'alice@example.com',
       },
     });
     console.log('Created new user:', newUser);

     // Fetch all users
     const allUsers = await prisma.user.findMany();
     console.log('All users:', allUsers);
   }

   main()
     .catch(e => {
       throw e;
     })
     .finally(async () => {
       await prisma.$disconnect();
     });
   ```

## Prisma Studio

To view and interact with your data via a graphical interface:

```bash
npx prisma studio
```

## Helpful Commands

- **Introspect your existing database schema:**

  ```bash
  npx prisma db pull
  ```

- **Update your Prisma schema with changes:**

  ```bash
  npx prisma generate
  ```

- **Reset the database and apply all migrations:**

  ```bash
  npx prisma migrate reset
  ```

## Conclusion

Prisma provides a powerful and flexible way to interact with your database. By following these steps, you can set up Prisma in your project, define your schema, manage migrations, and use Prisma Client to perform database operations.
