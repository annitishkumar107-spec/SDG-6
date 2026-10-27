# AquaCare setup

1. Connect the Supabase integration in Vercel/v0; its public URL and publishable key are already available to the app.
2. Run `supabase/migrations/001_aquacare.sql` in the connected Supabase SQL editor or MCP. It creates tables, the profile trigger, indexes, and RLS policies.
3. Create a private Storage bucket named `report-photos` and add policies allowing authenticated users to upload/read only paths prefixed by their user id. Keep the service-role key server-only.
4. Configure Supabase Auth email confirmation and add the preview and production callback URL `/auth/callback` to the allowed redirect URLs.
5. Set `NEXT_PUBLIC_MAP_TILE_URL` and select a geocoder. Nominatim is suitable for development only; use a provider with an API key and caching for production, respecting its usage policy.
6. Apply the first admin role only from a trusted server-side/database-admin procedure after verifying the person: `update public.profiles set role='admin' where id='<verified-auth-user-id>';` Never expose this as a public endpoint.
7. Test sign-up, confirmation, login, recovery, report persistence, RLS ownership, and staff-only operations before production deployment.

The application does not fabricate sources or claim reports were sent to municipal authorities. Empty source data is expected until an authorized moderator publishes verified records.
