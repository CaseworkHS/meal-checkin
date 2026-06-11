# HealthServe CWSS Meal Check-in System

## Setup steps (one time, ~20 minutes)

### Step 1 — Create Supabase database (free)
1. Go to https://supabase.com and sign up (free)
2. Create a new project — name it "healthserve-meals"
3. Wait 2 minutes for it to set up
4. Go to SQL Editor and run this:

```sql
create table meal_checkins (
  id uuid default gen_random_uuid() primary key,
  fin_last4 text not null,
  checkin_date date not null,
  checkin_time time,
  meal_number int,
  created_at timestamptz default now()
);

-- Allow anonymous inserts and reads (for the app)
alter table meal_checkins enable row level security;
create policy "Allow anonymous insert" on meal_checkins for insert with check (true);
create policy "Allow anonymous select" on meal_checkins for select using (true);
```

5. Go to Settings > API
6. Copy "Project URL" — this is YOUR_SUPABASE_URL
7. Copy "anon public" key — this is YOUR_SUPABASE_ANON_KEY

### Step 2 — Update config in both HTML files
In index.html and staff.html, replace:
- YOUR_SUPABASE_URL  →  your copied Project URL
- YOUR_SUPABASE_ANON_KEY  →  your copied anon key

### Step 3 — Deploy to GitHub Pages (free)
1. Create GitHub account at https://github.com
2. Create new repository — name it "meal-checkin" — make it PUBLIC
3. Upload index.html, staff.html, README.md
4. Go to repository Settings > Pages
5. Source: Deploy from branch > main > / (root) > Save
6. Your URL will be: https://YOUR-USERNAME.github.io/meal-checkin

### Step 4 — Generate QR code
1. Go to https://www.qr-code-generator.com (free)
2. Enter your URL: https://YOUR-USERNAME.github.io/meal-checkin
3. Download QR code as PNG
4. Print, laminate, display at Al-Rich counter

### Step 5 — Staff dashboard
Staff access: https://YOUR-USERNAME.github.io/meal-checkin/staff.html
Bookmark this on Daniel and Tanushree's phones.
Shows live redemptions, daily count, cap usage, export to CSV.

## Monthly maintenance — zero
The app runs forever. No changes needed month to month.
The database stores everything permanently.

## Viewing data as Excel
In Supabase dashboard > Table Editor > meal_checkins > Export to CSV
Open CSV in Excel. Done.

## If something breaks
The only thing that can break is Supabase (very rare) or GitHub Pages (very rare).
Both have status pages:
- https://status.supabase.com
- https://www.githubstatus.com

## What the app enforces automatically
- One meal per FIN last 4 per day
- Maximum 25 meals per day (edit DAILY_CAP in index.html to change)
- 12pm–5pm Monday–Friday only (edit MEAL_START_HOUR/MEAL_END_HOUR)
- Outside hours shows clear message in English and Bengali
- 
