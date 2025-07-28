# 💸 Refund API

API desenvolvida com **Node.js**, **TypeScript** e **Prisma** para gerenciar solicitações de **reembolso de despesas**. Este projeto conta com **autenticação JWT**, **upload de comprovantes**, **validação de dados**, **controle de acesso por cargo**, além de arquitetura modular e práticas modernas de desenvolvimento back-end.

Projeto prático desenvolvido por **Frederico Nakajima**.

---

## 📌 Funcionalidades

- 📁 Upload e validação de arquivos `.jpg`, `.jpeg`, `.png` (até 3MB)
- 🔒 Autenticação com **JWT**
- 👤 Controle de acesso por `role` (`employee` ou `manager`)
- 🔐 Middleware de proteção de rotas (`ensureAuthenticated`)
- 💾 Persistência com **Prisma + SQLite**
- 📤 Organização automática de arquivos em pastas `tmp/` e `uploads/`
- 📃 Tratamento centralizado de erros (AppError, ZodError)
- 📦 Validação com **Zod**
- 📄 Tipagem global de `request.user` com TypeScript

---

## ⚙️ Tecnologias utilizadas

- [Node.js](https://nodejs.org)
- [TypeScript](https://www.typescriptlang.org/)
- [Express](https://expressjs.com/)
- [Prisma ORM](https://www.prisma.io/)
- [SQLite](https://www.sqlite.org/)
- [JWT (jsonwebtoken)](https://jwt.io/)
- [Multer](https://github.com/expressjs/multer)
- [Bcrypt.js](https://github.com/dcodeIO/bcrypt.js)
- [Zod](https://github.com/colinhacks/zod)

---

## 🧱 Modelagem do banco (Prisma)

```prisma
model User {
  id        String   @id @default(uuid())
  name      String
  email     String   @unique
  password  String
  role      UserRole @default(employee)
  refunds   Refunds[]
  createdAt DateTime @default(now()) @map("created_at")
  updatedAt DateTime? @updatedAt @map("updated_at")
}

model Refunds {
  id        String   @id @default(uuid())
  name      String
  amount    Float
  category  Category
  filename  String
  userId    String   @map("user_id")
  user      User     @relation(fields: [userId], references: [id])
  createdAt DateTime @default(now()) @map("created_at")
  updatedAt DateTime? @updatedAt @map("updated_at")
}

enum UserRole {
  employee
  manager
}

enum Category {
  food
  others
  services
  transport
  accommodation
}
