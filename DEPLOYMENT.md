# Deployment Guide

This guide covers how to deploy the Hillsdale Community Foundation website to various platforms.

## 📋 Pre-deployment Checklist

Before deploying, ensure:

- [ ] All changes are committed and pushed to the main branch
- [ ] `npm run build` completes successfully locally
- [ ] All tests pass (if applicable)
- [ ] Content is up to date
- [ ] Environment variables are configured (if needed)

## 🎯 GitHub Pages (Recommended)

The site is pre-configured for GitHub Pages deployment with automated CI/CD.

### Setup Steps

1. **Enable GitHub Pages** in your repository:
   - Go to Settings > Pages
   - Source: "GitHub Actions"
   - The workflow will automatically deploy on pushes to `main`

2. **Update Configuration** (if needed):
   - Repository name should match the `basePath` in `next.config.js`
   - Current setting: `basePath: '/nextjs-demo'`
   - If your repo is named differently, update this value

3. **Deploy**:
   ```bash
   git add .
   git commit -m "Deploy website"
   git push origin main
   ```

4. **Access**: Your site will be available at:
   `https://[username].github.io/[repository-name]/`

### Automatic Deployment

The GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:
- Installs dependencies
- Builds the Next.js application
- Deploys to GitHub Pages
- Runs on every push to `main` branch

## 🌐 Alternative Deployment Options

### Vercel

1. **Connect Repository**:
   - Go to [vercel.com](https://vercel.com)
   - Import your GitHub repository
   - Vercel auto-detects Next.js

2. **Configure**:
   - Build Command: `npm run build`
   - Output Directory: `out`
   - Framework Preset: Next.js

3. **Environment Variables**: Add any required environment variables in the Vercel dashboard

### Netlify

1. **Connect Repository**:
   - Go to [netlify.com](https://netlify.com)
   - Connect your GitHub repository

2. **Build Settings**:
   - Build Command: `npm run build`
   - Publish Directory: `out`

3. **Deploy**: Automatic deployment on git push

### Traditional Web Hosting

1. **Build Locally**:
   ```bash
   npm run build
   ```

2. **Upload Files**:
   - Upload the entire `out/` directory to your web server
   - Ensure your web server can serve static files
   - Configure redirects for client-side routing (if needed)

## ⚙️ Configuration for Different Environments

### Environment-Specific Settings

The `next.config.js` is configured to handle different environments:

```javascript
const isDev = process.env.NODE_ENV === 'development';
const nextConfig = {
  // Only use basePath and assetPrefix for production
  ...(isDev ? {} : {
    basePath: '/nextjs-demo',
    assetPrefix: '/nextjs-demo/',
  }),
}
```

### Custom Domain

If deploying to a custom domain:

1. **Remove basePath** from `next.config.js`:
   ```javascript
   const nextConfig = {
     output: 'export',
     // Remove or comment out basePath and assetPrefix
     // basePath: '/nextjs-demo',
     // assetPrefix: '/nextjs-demo/',
   }
   ```

2. **Update GitHub Pages** (if using):
   - Add a `CNAME` file to the `public/` directory with your domain
   - Configure DNS records to point to GitHub Pages

## 📊 Monitoring Deployment

### GitHub Actions

Monitor deployment status:
- Go to Actions tab in your GitHub repository
- View the latest "Deploy to GitHub Pages" workflow
- Check logs for any errors

### Build Logs

Common build issues and solutions:

- **Missing dependencies**: Run `npm install` locally
- **TypeScript errors**: Run `npm run typecheck`
- **Build failures**: Check the Actions logs for specific errors

## 🔧 Troubleshooting

### Common Issues

1. **404 on GitHub Pages**:
   - Ensure `basePath` matches your repository name
   - Check that files are in the correct `out/` directory

2. **CSS not loading**:
   - This should be fixed with the current configuration
   - Verify `assetPrefix` is set correctly for production

3. **Images not displaying**:
   - Ensure images are in the `public/` directory
   - Use relative paths starting with `/`

4. **Build timeouts**:
   - Large dependency installations may timeout
   - Consider using `npm ci` instead of `npm install`

### Getting Help

If you encounter issues:
1. Check the GitHub Actions logs
2. Test the build locally with `npm run build`
3. Verify all environment variables are set
4. Create an issue in the repository

## 🔄 Rolling Back

To rollback a deployment:

1. **GitHub Pages**: 
   - Revert the commit and push
   - Or deploy a previous working commit

2. **Vercel/Netlify**:
   - Use their dashboard to rollback to a previous deployment

3. **Manual hosting**:
   - Re-upload a previous build's `out/` directory

---

For additional help with deployment, consult the [Next.js deployment documentation](https://nextjs.org/docs/deployment).
