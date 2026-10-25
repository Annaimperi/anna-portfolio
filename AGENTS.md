<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.
<!-- LOVABLE:END -->

- All server code writes to the owner's external PostgreSQL (Timeweb) only through `db()` in `src/lib/db.server.ts`, credentials from TIMEWEB_DB_* env vars — the user requires their own DB, not Lovable Cloud.
- Timeweb DB connections use `ssl: false` plus connect/query timeouts — Timeweb's self-signed cert makes TLS hang forever in the Worker runtime (works in dev on Node, so only the published site breaks).
- Site is multi-page: shared header/footer live in `src/routes/__root.tsx`; services, cases, posts, FAQ and prices are data in `src/lib/*.ts` so pages and the sitemap stay in sync.
- Every route sets `staticData.sitemap`; `/sitemap.xml` derives static URLs from the router and adds slug pages from the same data lists.
- AI seller: chat via createServerFn in src/lib/ai.functions.ts (Gateway SDK); owner-editable bot settings live in Timeweb table portfolio_schema.ai_settings, edited at hidden /admin/ai-training behind ADMIN_PASSWORD + SESSION_SECRET cookie session — keeps the owner's DB as the only store.
