# Nyota Herbal & Natural Wellness — Marketplace v2

React + TypeScript + Vite multi-seller storefront with Supabase Auth/Postgres/Storage and Vercel configuration.

## Included
- Public herbal/natural product catalogue with search/category filters and WhatsApp product enquiries.
- Admin product create/edit/delete, image upload (JPEG/PNG/WebP, max 5 MB), description, category, price and stock.
- Three roles: `super_admin`, `admin`, `customer`.
- Tenant isolation: product owner UUID is assigned from the signed-in user; ordinary admins can only manage their own products. Super admin has platform-wide product/profile/audit visibility via RLS.
- Super-admin account overview, platform inventory and security/upload activity log. Password change for signed-in user.
- TikTok showcase and WhatsApp catalogue links from the request.

## Setup required before live use
1. Install Node.js 20 LTS+ and create a Supabase project at https://supabase.com.
2. Run the complete `supabase/schema.sql` in Supabase SQL Editor.
3. Copy `.env.example` to `.env`. Set `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY` and a unique temporary `INITIAL_SUPER_ADMIN_PASSWORD` of 12+ characters.
4. Never commit `.env`; never put the service-role key in a `VITE_` variable.
5. Set separate, unique 12+ character temporary passwords in `INITIAL_SUPER_ADMIN_PASSWORD` and `INITIAL_ADMIN_PASSWORD`.
6. Run `npm install`, then `npm run seed:admins` from the project root. The script automatically loads local `.env`, creates missing Auth users, updates passwords for these two configured accounts if they already exist, and upserts their role profiles. Run it only against your intended Supabase project.
7. Sign in to both accounts and change their temporary passwords immediately. Create further admin accounts only through trusted Supabase Auth/admin tooling and give each their own `business_name` and unique credentials. Never allow browser users to assign roles.
8. Run locally with `npm run dev`; production build with `npm run build`.

## Vercel deployment
Import the repo into https://vercel.com/new, select Vite, build command `npm run build`, output `dist`. Configure only `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in frontend environment variables. Never expose service-role key to browser.

## Tenant model and security
`products.owner_id` is enforced by Row Level Security. Admin 1 cannot edit Admin 2's products; each admin has an independent business profile. Super admin can review profiles, products and audit logs. Product photo objects are stored under a folder matching the uploader UUID. Deleting a product does not yet automatically remove its stored image object. Profile `status` alone does not ban a Supabase Auth session. For a scalable production system, implement protected server-side Edge Functions for admin invites, auth suspension, password resets, role changes, account editing and secure image cleanup; do not expose service-role secrets in React.

## Current scope limitations
This is a deployable project foundation, not an already-hosted live website. The admin account screen is monitoring-focused; auth-level suspension, invitation/reset workflows, MFA enforcement, persistent business-profile editing, order/payment processing, sales/revenue analytics, refunds, delivery, email notifications, WhatsApp Business API automation, rate limiting/CAPTCHA and storage cleanup need dedicated server-side modules before broad commercial launch. Public enquiries are captured in the database, but assigning an enquiry to the correct seller needs a product/seller-aware enquiry flow before multi-seller CRM use.

## Herbal product compliance
Use accurate ingredients/botanical names, source, quantity, directions, warnings and batch/expiry data where relevant. Avoid unsupported disease-cure claims. Verify applicable Kenyan requirements before selling herbal medicines, supplements or therapeutic products. This marketplace is not medical advice.

TikTok: https://vm.tiktok.com/ZMkVArvD7/
WhatsApp catalogue: https://wa.me/c/254722587764
