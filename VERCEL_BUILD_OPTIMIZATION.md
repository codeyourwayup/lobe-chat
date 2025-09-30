# Vercel Build Optimization Guide

## Changes Made

### 1. Fixed Next.js Configuration Warning
- **Issue**: `experimental.serverComponentsExternalPackages` is deprecated in Next.js 15.3.5
- **Fix**: Moved `sharp` from `experimental.serverComponentsExternalPackages` to top-level `serverExternalPackages`
- **Location**: `next.config.ts` line 284

### 2. Enhanced Memory Optimization
Added additional webpack optimizations to reduce memory usage during build:
- Set `maxInitialRequests: 25` to limit parallel chunk loading
- Set `minSize: 20000` to prevent creating too many small chunks
- Configured minimizer settings for production builds

## Vercel Environment Variables to Set

To further optimize the build on Vercel, add these environment variables in your Vercel project settings:

### Required
```bash
NODE_OPTIONS=--max-old-space-size=6144
```

### Recommended for Memory Issues
```bash
# Disable PWA during build if not needed
DISABLE_PWA=1

# Disable optimize imports if causing issues
DISABLE_OPTIMIZE_IMPORTS=1

# Use production mode
NODE_ENV=production
```

## Vercel Project Settings

### Build & Development Settings
- **Framework Preset**: Next.js
- **Build Command**: `bun run build` (default)
- **Output Directory**: `.next` (default)
- **Install Command**: `pnpm install` (recommended)

### Performance Settings
Consider upgrading to a higher Vercel plan if OOM errors persist:
- **Hobby Plan**: 1024 MB RAM
- **Pro Plan**: 3008 MB RAM (recommended for this project)
- **Enterprise Plan**: Custom limits

## Alternative Solutions

### Option 1: Reduce Build Complexity
Temporarily disable features during build:
```bash
DISABLE_PWA=1
REACT_SCAN_MONITOR_API_KEY=  # Remove if set
```

### Option 2: Use Docker Build
For consistent builds with more control:
```bash
npm run build:docker
```

### Option 3: Split Build Process
If the issue persists, consider:
1. Building locally or in CI/CD
2. Deploying pre-built artifacts to Vercel
3. Using Vercel's prebuilt output API

## Monitoring Build Performance

Check build logs for:
- Memory usage warnings
- Webpack compilation times
- Large bundle sizes

Use Next.js bundle analyzer:
```bash
ANALYZE=true npm run build
```

## Additional Resources
- [Vercel Build Troubleshooting](https://vercel.link/troubleshoot-build-errors)
- [Next.js Memory Optimization](https://nextjs.org/docs/messages/invalid-next-config)
- [Webpack Memory Management](https://webpack.js.org/configuration/other-options/#cache)
