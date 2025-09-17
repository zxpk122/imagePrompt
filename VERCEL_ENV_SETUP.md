# Vercel Environment Variables Setup Guide

## Build Error Resolution

If you're seeing this error during Vercel deployment:
```
❌ Invalid environment variables: {
  NEXTAUTH_SECRET: [ 'Required' ],
  GITHUB_CLIENT_ID: [ 'Required' ],
  GITHUB_CLIENT_SECRET: [ 'Required' ],
  STRIPE_API_KEY: [ 'Required' ],
  STRIPE_WEBHOOK_SECRET: [ 'Required' ],
  NEXT_PUBLIC_APP_URL: [ 'Required' ]
}
```

This guide will help you configure all required environment variables in Vercel.

## Required Environment Variables

### 1. Application Configuration
- **NEXT_PUBLIC_APP_URL**: Your deployed application URL
  - Example: `https://your-app.vercel.app`
  - Set this to your actual Vercel deployment URL

### 2. Authentication (NextAuth.js)
- **NEXTAUTH_SECRET**: Random secret for JWT encryption
  - Generate: `openssl rand -base64 32`
  - Or use: `https://generate-secret.vercel.app/32`

### 3. GitHub OAuth (Required for authentication)
- **GITHUB_CLIENT_ID**: GitHub OAuth App Client ID
- **GITHUB_CLIENT_SECRET**: GitHub OAuth App Client Secret

#### Setting up GitHub OAuth:
1. Go to GitHub Settings → Developer settings → OAuth Apps
2. Click "New OAuth App"
3. Fill in:
   - Application name: `Your App Name`
   - Homepage URL: `https://your-app.vercel.app`
   - Authorization callback URL: `https://your-app.vercel.app/api/auth/callback/github`
4. Copy the Client ID and generate a Client Secret

### 4. Stripe Configuration (Required for payments)
- **STRIPE_API_KEY**: Stripe Secret Key (starts with `sk_`)
- **STRIPE_WEBHOOK_SECRET**: Stripe Webhook Endpoint Secret (starts with `whsec_`)

#### Setting up Stripe:
1. Go to Stripe Dashboard → Developers → API keys
2. Copy the "Secret key" (use test key for development)
3. Go to Webhooks → Add endpoint
4. Endpoint URL: `https://your-app.vercel.app/api/webhooks/stripe`
5. Select events or use "Select all events"
6. Copy the "Signing secret"

## How to Set Environment Variables in Vercel

### Method 1: Vercel Dashboard
1. Go to your project in Vercel Dashboard
2. Navigate to Settings → Environment Variables
3. Add each variable:
   - Name: Variable name (e.g., `NEXTAUTH_SECRET`)
   - Value: The actual value
   - Environment: Select "Production", "Preview", and "Development" as needed
4. Click "Save"

### Method 2: Vercel CLI
```bash
# Install Vercel CLI if not already installed
npm i -g vercel

# Login to Vercel
vercel login

# Set environment variables
vercel env add NEXTAUTH_SECRET
vercel env add GITHUB_CLIENT_ID
vercel env add GITHUB_CLIENT_SECRET
vercel env add STRIPE_API_KEY
vercel env add STRIPE_WEBHOOK_SECRET
vercel env add NEXT_PUBLIC_APP_URL
```

## Quick Setup Checklist

- [ ] Set `NEXT_PUBLIC_APP_URL` to your Vercel deployment URL
- [ ] Generate and set `NEXTAUTH_SECRET`
- [ ] Create GitHub OAuth App and set `GITHUB_CLIENT_ID` + `GITHUB_CLIENT_SECRET`
- [ ] Get Stripe API key and set `STRIPE_API_KEY`
- [ ] Create Stripe webhook and set `STRIPE_WEBHOOK_SECRET`
- [ ] Redeploy your application

## After Setting Variables

1. **Redeploy**: Trigger a new deployment after adding all environment variables
2. **Verify**: Check that the build completes successfully
3. **Test**: Ensure authentication and payment features work correctly

## Troubleshooting

### Build Still Fails?
- Ensure all variable names are exactly as shown (case-sensitive)
- Check that values don't have extra spaces or quotes
- Verify GitHub OAuth callback URL matches your deployment URL
- Confirm Stripe webhook endpoint URL is correct

### Need Help?
- Check the original `.env.example` file for reference values
- Verify your third-party service configurations (GitHub, Stripe)
- Ensure your Vercel deployment URL is accessible

## Security Notes

- Never commit actual environment variables to your repository
- Use different values for development, preview, and production environments
- Regularly rotate secrets, especially `NEXTAUTH_SECRET`
- Use Stripe test keys for non-production environments