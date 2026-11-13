# OneGarden

A plant shop built with React, Vite, Supabase, Express, and Razorpay.

## Start locally

1. Install Node.js (current LTS) and run `npm install`.
2. `.env` is already present in this project. Add the missing server-only values from `.env.example` to it; do not overwrite your existing values. Keep secret keys out of `VITE_*` variables and never commit `.env`.
3. In Supabase, open SQL Editor and run `supabase/schema.sql`. Create an administrator account under Authentication, then run the final admin insert with that account's email.
4. In Supabase Storage, create a **public** bucket named `plant-images`. The schema script grants image access to visitors and write access only to admins.
5. Add your Supabase URL and anon/publishable key to the `VITE_*` entries. Add the Supabase service role key to `SUPABASE_SERVICE_ROLE_KEY` (server only).
6. Add Razorpay test/live key ID and secret to the server variables. Use test keys while developing.
7. In one terminal run `npm run dev`; in another run `npm run server`.

The shop catalog reads products from Supabase. The SQL setup adds the four starter plants if they are not already present. Checkout recalculates item prices and stock on the server; the browser never decides the charge amount. Confirmed Razorpay payments are saved to `orders` and reduce stock atomically. Contact form messages are saved for admins in `contact_messages` and are visible under `/admin/messages`.

## Admin access

Visit `/admin/login` and sign in with the Supabase Auth account added to `onegarden_admins`. Admin product routes require both a valid session and the database admin role. Do not use the temporary open policies in `admin-products-development-policies.sql` in production; the schema setup removes their common policy names and installs admin-only rules.

## Before publishing

Deploy the Vite site and the Express server to hosting that supports Node. Set production environment variables on the server, set `CLIENT_ORIGIN` to the exact site origin, and set `VITE_PAYMENT_SERVER_URL` to the deployed API URL before building the frontend. Configure Razorpay's webhook and business account in the Razorpay dashboard, and add a real support email, phone number, delivery, refund, and privacy policy for your business. Do not publish until a complete test payment and refund flow has been checked using Razorpay test mode.

Useful commands: `npm run build`, `npm run lint`, `npm run server`.


