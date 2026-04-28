# Contributing to GIMSOI AI Agent

Thank you for helping build the intelligence layer! To maintain high engineering standards, please follow this workflow.

## 🛠 The Workflow
1. **Branch out:** `git checkout -b feature/your-feature`
2. **Commit:** Use conventional messages (e.g., `feat: add risk logic`).
3. **Verify:** Run `npm install` in `backend/` to ensure no dependency breaks.
4. **Push:** `git push origin feature/your-feature`
5. **PR:** Open a Pull Request against `main`.

## 🛡 Security & Safety
- **Secrets:** Never commit `.env` files. Use GitHub Secrets for API keys.
- **Validation:** Every new API output must be defined in a Zod schema in `src/schemas/`.
- **RBAC:** Ensure any new routes use the `rbacMiddleware`.
