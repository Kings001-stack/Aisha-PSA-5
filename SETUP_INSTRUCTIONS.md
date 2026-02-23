# Opal CMS - Database Setup Guide

## Step 1: Create Database Tables

Follow these steps to set up your Supabase database:

1. **Open Supabase Dashboard**
   - Go to https://supabase.com/dashboard
   - Select your Opal project

2. **Navigate to SQL Editor**
   - Click on "SQL Editor" in the left sidebar
   - Click "New Query"

3. **Copy and Paste the SQL Migration**
   - Copy the entire contents of `/scripts/001_create_tables.sql`
   - Paste it into the SQL Editor
   - Click "Run" button (or press Ctrl+Enter)

4. **Verify Tables Were Created**
   - Go to "Table Editor" in the left sidebar
   - You should see these tables: admins, services, projects, blog_posts, testimonials, bookings, settings

## Step 2: Create an Admin User Account

After the tables are created, follow these steps to create your first admin account:

### Option A: Create Admin via Supabase Dashboard (Recommended)

1. **Go to Authentication**
   - In Supabase Dashboard, click "Authentication" in left sidebar
   - Click "Users" tab

2. **Add New User**
   - Click "Invite user" button
   - Enter your email address
   - Click "Send invite"
   - Check your email for the invite link
   - Click the link and set your password (must follow the requirements: 8-20 characters, letters, numbers, and symbols)

3. **Add Admin Role in Database**
   - Go to "SQL Editor" 
   - Create a new query
   - Run this SQL command (replace `your-user-id` with your actual user ID):

```sql
INSERT INTO public.admins (id, first_name, last_name, role)
VALUES ('your-user-id', 'Your', 'Name', 'admin');
```

   - To find your user ID: Go to Authentication → Users, click your user, and copy the ID from the URL or user details

### Option B: Create Admin via SQL (If you have user ID)

If you already know your Supabase user ID, run this SQL query:

```sql
INSERT INTO public.admins (id, first_name, last_name, role)
VALUES ('YOUR_SUPABASE_USER_ID_HERE', 'First', 'Last', 'admin');
```

## Step 3: Test the Login

1. Go to http://localhost:3000/admin/login
2. Enter your email and password
3. You should be logged in to the admin dashboard

## Password Requirements

Example valid password: `Admin@123456`

## Step 4: Preparing for Hosting

To deploy the application (e.g., to Vercel), ensure you set these environment variables in your hosting provider's dashboard:

1. **NEXT_PUBLIC_SUPABASE_URL**: Your Supabase Project URL.
2. **NEXT_PUBLIC_SUPABASE_ANON_KEY**: Your Supabase Anonymous Key.
3. **SUPABASE_SERVICE_ROLE_KEY**: Your Supabase Service Role Key (Found in Project Settings -> API). **Warning**: Keep this secret!

### Note on Build Logs
When running `npm run build`, you may see logs like `[AdminLayout] No session detected`. This is **normal**. It happens because the build process tries to pre-render the admin pages without a user logged in. The build is successful as long as it finishes with `Compiled successfully`.

## Troubleshooting

### "Failed to fetch" or "Already Used" errors
- **Solution**: We have implemented a "Passive Proxy" to prevent these. If you still see them, **Clear your browser cookies** and restart your browser.

### "User not found in admins table"
- **Solution**: You need to insert your user ID into the admins table using the SQL command above

### "Invalid password" 
- **Solution**: Password must meet the requirements above (8-20 chars, letters, numbers, symbols)

### "Invalid email"
- **Solution**: Email must be in valid format (example@domain.com)

## Next Steps

Once logged in, you can:
- Manage projects/portfolio
- Create and publish blog posts
- Manage testimonials
- View and respond to client bookings
- Configure website settings
