# District / Civil Hospital, Lunawada — Website & CMS

This project is based on the supplied `index(2).html`. It preserves its React renderer, CSS, navigation, gallery and scroll effects. The original 17 embedded media files are extracted into `public/assets`. The existing data and the previously hardcoded clinical teams, specialities, FAQs and contact fields are now editable.

## Cloudflare deployment (existing Worker)

Target: `floral-pond-974d` (https://floral-pond-974d.itcell-meet.workers.dev/). Admin: `/admin` after deployment. Do not deploy over the College project. Before updating production, save the previous Worker version ID from Deployments; test in a preview or separate test Worker first.

1. In Cloudflare, create a D1 database called `mahisagar-hospital-cms`. Open its Console and run the three CREATE TABLE statements from `schema.sql`.
2. Use the existing KV namespace `mahisagar-hospital-content` (ID `2b8083c52948400aa56525f86e59e2b3`) if it is still present in your account; otherwise create a new KV namespace.
3. Deploy the module Worker (`src/worker.js`, with `src/defaults.js` and `src/admin-ui.js`) together with the entire `public` directory as Workers Static Assets. Configure assets binding `ASSETS`, and enable `run_worker_first` so `/content.js`, `/admin`, and APIs reach the Worker. Set D1 binding `DB` to the database and KV binding `MEDIA` to the namespace. `deployment-settings.json` contains these settings in the Worker configuration format; substitute the actual DB ID. It is a reference, not an automatically loaded CLI config.
4. Use the Cloudflare CLI `cf` to configure/deploy this project. Consult `cf cli search "deploy Worker with static assets and D1 and KV bindings"` and the official CLI docs (https://developers.cloudflare.com/cf/agents/). Alternatively use the deployment workflow of your existing Worker project, transferring the settings into its existing config. Do not use a static-assets-only upload: the backend is required.
5. In the Worker Settings > Variables and Secrets, add `ADMIN_USER` (text, `admin` by default) and `ADMIN_PASSWORD` (Secret, choose a random password of at least 16 characters). Never put the password into HTML, public files or source. Redeploy settings if required. No default password is shipped. To rotate it, change the Cloudflare secret; existing sessions become invalid.
6. Open `/admin`, sign in, edit a harmless draft, confirm public content has not changed, publish, then confirm public content updates. Test logout and verify `/api/admin/data` returns HTTP 401 without a session. Use Cloudflare rollback if any website behavior regresses.

## Included
- Eight existing pages with editable titles, menus, descriptions and ordered content cards.
- Brand/logo, hero video/poster, footer text, doctor/clinical-team profiles, speciality lists, FAQs and contact details.
- Draft and publish, upload images and MP4 up to 20 MiB (KV); external HTTPS URLs supported. Public uploaded media is publicly accessible; do not upload confidential patient information. Large videos should use dedicated video storage. Unreferenced uploads are not automatically deleted.
- Server-side password verification, D1-backed session tokens stored as hashes, one-hour HttpOnly/Secure/SameSite cookies, request origin checks, database-backed IP login limit and upload type checks. One administrator account. Password changes are through Cloudflare settings, not the CMS. Some fixed UI button labels and CSS remain code-managed. No draft visual preview or multi-user roles in this version.

## Testing
Run `npm run check` for syntax checks. Cloudflare binding integration and browser rendering must be tested in a real deployment. No live deployment is included in this ZIP.
