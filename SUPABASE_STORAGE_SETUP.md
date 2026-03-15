# Supabase Storage Setup Guide

Your app uses the Supabase storage bucket `sem6_fixmycity` to store complaint images. Supabase offers **1GB of FREE storage** on their free tier!

## ✅ Good News: Your Bucket Already Exists!

I can see you already have the `sem6_fixmycity` bucket created and set as PUBLIC. Great!

Now you just need to verify the storage policies are set up correctly to allow uploads.

## Step-by-Step Setup

### 1. Access Your Supabase Dashboard

1. Go to [https://supabase.com](https://supabase.com)
2. Sign in to your account
3. Select your project (the one with URL: `hpjosagppcwwrtoxbreb.supabase.co`)

### 2. ~~Create the Storage Bucket~~ ✅ Already Done!

Your bucket `sem6_fixmycity` is already created and marked as PUBLIC. Skip to step 3!

### 3. Set Up Storage Policies (Important!)

After creating the bucket, you need to set up policies to allow uploads and public access:

#### Enable Public Read Access

1. Click on your `sem6_fixmycity` bucket
2. Go to the **"Policies"** tab
3. Click **"New policy"**
4. Select **"For full customization"** → **"SELECT"** operation
5. Fill in:
   - **Policy name**: `Public Read Access`
   - **Allowed operation**: SELECT
   - **Target roles**: `public`
   - **Policy definition**: Use this SQL:
   ```sql
   true
   ```
6. Click **"Review"** then **"Save policy"**

#### Enable Authenticated Upload Access

1. Click **"New policy"** again
2. Select **"For full customization"** → **"INSERT"** operation
3. Fill in:
   - **Policy name**: `Authenticated Upload`
   - **Allowed operation**: INSERT
   - **Target roles**: `authenticated`
   - **Policy definition**: Use this SQL:
   ```sql
   (bucket_id = 'sem6_fixmycity')
   ```
4. Click **"Review"** then **"Save policy"**

#### Enable Authenticated Delete Access (Optional)

1. Click **"New policy"** again
2. Select **"For full customization"** → **"DELETE"** operation
3. Fill in:
   - **Policy name**: `Authenticated Delete`
   - **Allowed operation**: DELETE
   - **Target roles**: `authenticated`
   - **Policy definition**: Use this SQL:
   ```sql
   (bucket_id = 'sem6_fixmycity')
   ```
4. Click **"Review"** then **"Save policy"**

### 4. Verify Setup

To verify everything is working:

1. Try uploading a test image through your app
2. Check the Storage tab in Supabase to see if the image appears
3. Try accessing the image URL - it should be publicly viewable

## Free Tier Limits

Supabase Free Tier includes:
- ✅ **1GB storage** (plenty for civic reports)
- ✅ **2GB bandwidth per month**
- ✅ Unlimited requests

For a civic reporting app, this should be more than enough!

## Troubleshooting

### Error: "Bucket not found"
- Make sure you created a bucket named exactly `sem6_fixmycity`
- Bucket names are case-sensitive

### Error: "new row violates row-level security policy"
- You need to set up the storage policies (see Step 3 above)
- Make sure the INSERT policy allows authenticated users

### Images not visible
- Ensure the bucket is marked as **Public**
- Check that the SELECT policy allows public access

### Storage quota exceeded
- Check your usage in Supabase Dashboard → Settings → Billing
- Delete old/unused images
- Upgrade to Pro plan if needed ($25/month with 100GB storage)

## Alternative: Image Compression

To save storage space, you can compress images before uploading. This is already handled in the frontend with a 5MB limit per image.

## Need Help?

- Supabase Storage Docs: [https://supabase.com/docs/guides/storage](https://supabase.com/docs/guides/storage)
- Supabase Community: [https://github.com/supabase/supabase/discussions](https://github.com/supabase/supabase/discussions)
