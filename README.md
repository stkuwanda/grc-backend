# 🛡️ GRC Backend

A modular, audit-ready backend built with **Express + TypeScript**, designed for compliance platforms that demand clarity, resilience, and scale.

---

## 🚀 Features

- TypeScript strict mode for type safety  
- Modular feature folders (`assets`, `consents`, `risks`, `incidents`, `compliance`, `audit`, `users`)  
- Config-driven RBAC enforcement via middleware  
- Centralized error handling with `AppError` and global error middleware  
- Audit trail emitter for security-sensitive actions  
- Unit & integration tests (Jest + Supertest)  
- Dev-ramp CLI-ready structure  
- Ready for Prisma, Drizzle, or raw SQL integration  

---

## 📁 Folder Structure

```
src/
├── config/         # Environment, database, and logger setup
├── core/           # RBAC logic, error classes, audit emitter
├── middlewares/    # Authentication, validation, and error handler
├── modules/        # Feature folders
│   ├── assets/
│   ├── consents/
│   ├── risks/
│   ├── incidents/
│   ├── compliance/
│   ├── audit/
│   └── users/
├── utils/          # Helper functions and response wrappers
├── tests/          # Unit and integration tests
├── server.ts       # App setup and route registration
└── index.ts        # Entry point
```

---

## 🏁 Getting Started

### Prerequisites

- Node.js >= 16  
- npm  
- PostgreSQL (or your preferred database)  

### Installation

```bash
git clone https://github.com/your-org/govhub-backend.git
cd govhub-backend
npm install
```

### Environment

Copy `.env.example` to `.env` and configure:

```env
PORT=4000
DATABASE_URL=postgres://user:pass@localhost:5432/govhub
JWT_SECRET=your-jwt-secret
```

### Run the App

- Development (with hot reload):  
  ```bash
  npm run dev
  ```
- Production build + start:  
  ```bash
  npm run build
  npm start
  ```

---

## 🧑‍💻 Scripts

| Command          | Description                             |
| ---------------- | --------------------------------------- |
| `npm run dev`    | Run dev server with ts-node-dev         |
| `npm run build`  | Compile TypeScript to `/dist`           |
| `npm start`      | Run compiled server                     |
| `npm run lint`   | Run ESLint                              |
| `npm test`       | Run Jest test suite                     |
| `npm run format` | Format code with Prettier               |

---

## 🔐 RBAC Enforcement

Define permissions in `src/core/rbac.ts`:

```ts
export const permissions = {
  assets: {
    view: ['admin', 'viewer'],
    edit: ['admin']
  },
  risks: {
    view: ['admin', 'dpo'],
    create: ['admin']
  },
  // ...
};
```

Protect routes with middleware:

```ts
router.get(
  '/',
  requirePermission('assets', 'view'),
  assetController.getAll
);
```

---

## 📊 Audit Trail

Emit an audit event for sensitive operations:

```ts
import { emitAudit } from '../core/audit';

await assetService.delete(id);
emitAudit({
  actor: req.user.id,
  action: 'DELETE_ASSET',
  target: id,
  timestamp: Date.now()
});
```

---

## 🧪 Testing

- Unit tests for services  
- Integration tests for routes (using Supertest)  
- RBAC enforcement and error handling tests  

Run all tests:

```bash
npm test
```

Test files live alongside modules and in `src/tests/`.

---

## 📌 Contribution Guide

1. Fork the repository  
2. Create a feature branch (`feat/your-feature`)  
3. Follow the module pattern:  
   - `controller.ts` — HTTP handlers  
   - `service.ts` — Business logic  
   - `routes.ts` — Express router  
   - `model.ts` — Database schema or ORM mapping  
   - `types.ts` — DTOs & interfaces  
4. Write tests and ensure coverage  
5. Submit a Pull Request with a clear description  

---

## 📚 License

This project is licensed under the MIT License.

---

## 👥 Maintainers

- **Simbarashe Kuwanda** — Lead Architect & Technical Strategist  
- **Contributors** — Welcome!  