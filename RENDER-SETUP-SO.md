# GHDD Assessment Pro V2 — Render deployment

This package is a separate deployment copy. It does not modify the existing Railway app or its database.

## Important data separation
- Use a NEW PostgreSQL database for V2. Do not paste the old Railway `DATABASE_URL` unless you intentionally want both apps to share the same data.
- This app stores uploaded book PDFs and OCR content in PostgreSQL tables, so configure `BOOKS_DATABASE_URL` to the V2 database, or leave it unset if the app should use `DATABASE_URL` for books too.
- Never commit API keys or database passwords to GitHub.

## Deploy
1. Push all files in this folder to the NEW GitHub repository.
2. In Render, choose **New + → Blueprint** and connect that repository.
3. Render will read `render.yaml`. If Blueprint is unavailable, create a **New Web Service**:
   - Runtime: Node
   - Build command: `npm install`
   - Start command: `npm start`
   - Health check path: `/api/health`
4. Add environment variables in Render:
   - `GEMINI_API_KEY`: your new Google AI Studio key
   - `DATABASE_URL`: connection string for the NEW PostgreSQL database
   - `BOOKS_DATABASE_URL`: optional; set to the same new database URL if you want book data in that database
   - `ADMIN_PASSWORD`: set a strong private password if the app uses it
   - `GEMINI_MODEL`: `gemini-2.5-flash` or another model enabled for your API key
5. Deploy, then open `/api/health` on the new Render URL to check service health.

## Before using real school data
- Upload one test PDF and verify OCR, book retrieval, exam generation, and Word export.
- Confirm the new V2 database is separate from the old Railway database.
- Render Free web services may sleep when idle. Do not use local disk as permanent storage; this app's book/PDF storage is configured through PostgreSQL.
- Keep the old Railway service and database until V2 has been fully tested.

## Notes
This setup file does not create the Render service or database automatically. You must create/connect the database and add secrets in the Render dashboard.
