A real-time collaborative expense and payment tracker (like Splitwise meets Razorpay Wallet) built using Next.js (App Router) + Prisma + PostgreSQL + WebSockets, showcasing core DBMS concepts like transactions, indexing, normalization, constraints, concurrency control, and recovery — all in one real-world project.
| Layer                 | Tech                                                                    |
| --------------------- | ----------------------------------------------------------------------- |
| **Frontend**          | Next.js (App Router) + TypeScript + Tailwind + Zustand or Redux Toolkit |
| **Backend**           | Next.js API routes (server actions / REST endpoints)                    |
| **Database**          | PostgreSQL                                                              |
| **ORM**               | Prisma                                                                  |
| **Real-time Updates** | WebSockets (via Socket.io or Pusher)                                    |
| **Auth**              | NextAuth.js (Google, GitHub, Email)                                     |
| **Deployment**        | Vercel (frontend) + Railway/NeonDB (database)                           |

Core DBMS Concepts in Action
| Concept                   | Implementation in LedgerLink                                                          |
| ------------------------- | ------------------------------------------------------------------------------------- |
| **Transactions**          | Money transfers, expense settlements use `prisma.$transaction()`                      |
| **Indexing**              | Indexed queries on userId, groupId, createdAt for analytics                           |
| **Normalization (3NF)**   | Users, Wallets, Groups, Expenses, Transactions are separate tables with relationships |
| **Constraints**           | Foreign keys, unique wallet per user, balance ≥ 0 enforced via `CHECK` constraint     |
| **Concurrency Control**   | Transaction isolation level: `SERIALIZABLE` to prevent race conditions                |
| **Recovery**              | Pending transactions logged and rolled back on startup                                |
| **Triggers (Middleware)** | Prisma middleware to auto-update balances on transaction insert                       |
| **Query Optimization**    | Compare indexed vs. non-indexed queries with `EXPLAIN ANALYZE`                        |

System Architecture Overview
Client (Next.js + WebSocket)
         │
         ▼
Next.js API Routes (server actions)
         │
         ▼
     Prisma ORM
         │
         ▼
     PostgreSQL DB
         │
         ▼
Real-time Events (Socket.io or Pusher)

Real-time layer:

When any transaction or balance update occurs, the backend emits a balanceUpdate event via WebSocket.

All clients subscribed to that user/group receive instant updates (no refresh needed).
