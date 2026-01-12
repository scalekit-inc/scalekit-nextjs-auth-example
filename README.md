## Next.js Scalekit Authentication Example

A simple Next.js app that shows how to add secure sign-in with Scalekit (OIDC). You can use it as a starting point or as a reference to integrate enterprise-grade authentication.

What this example includes:

- The app signs users in with Scalekit using the OpenID Connect (OIDC) authorization flow.
- The `/dashboard` page is protected and redirects unauthenticated users to the login flow.
- The configuration shows how to register an OAuth 2.0 client and wire login, callback, and logout endpoints.
- The pages use Bootstrap classes so pages render well on desktop and mobile.
- After login, the dashboard displays selected ID token claims to demonstrate how to access user information.

### Prerequisites

- Node.js 18.0 or later is installed.
- npm or yarn is installed.
- You have a Scalekit account with an OIDC application. [Sign up](https://app.scalekit.com/)

## 🛠️ Quick start

### Configure Scalekit

Pick one method below.

_Method A_ — .env.local file (recommended for local dev):

Create or update `.env.local` in the project root:

```env
# Replace placeholders with your values
SCALEKIT_ENV_URL=https://your-env.scalekit.io
SCALEKIT_CLIENT_ID=YOUR_CLIENT_ID
SCALEKIT_CLIENT_SECRET=YOUR_CLIENT_SECRET
SCALEKIT_REDIRECT_URI=http://localhost:3000/auth/callback

# Optional server config
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

_Method B_ — environment variables:

```bash
export SCALEKIT_ENV_URL=https://your-env.scalekit.io
export SCALEKIT_CLIENT_ID=YOUR_CLIENT_ID
export SCALEKIT_CLIENT_SECRET=YOUR_CLIENT_SECRET
export SCALEKIT_REDIRECT_URI=http://localhost:3000/auth/callback
export NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Important:

- Never commit secrets to source control.
- Ensure the redirect URI exactly matches what is configured in Scalekit.

### Build and run

```bash
# Install dependencies
npm install
# or
yarn install
# or
pnpm install

# Run the development server
npm run dev
# or
yarn dev
# or
pnpm dev
```

The application will start at `http://localhost:3000`

### Setup Scalekit

To find your required values:

1.  Visit [Scalekit Dashboard](https://app.scalekit.com) and proceed to _Settings_

2.  Copy the API credentials

    - **Environment URL** (e.g., `https://your-env.scalekit.dev`)
    - **Client ID**
    - **Client Secret**

3.  Authentication > Redirect URLs > Allowed redirect URIs:
    - Add `http://localhost:3000/auth/callback` (no trailing slash)
    - Optionally add `http://localhost:3000` as a post-logout redirect

### Application routes

| Route                            | Description                 | Auth required |
| -------------------------------- | --------------------------- | ------------- |
| `/`                              | Home page with login option | No            |
| `/login`                         | Custom login page           | No            |
| `/auth/callback`                 | OIDC callback (API route)   | No            |
| `/dashboard`                     | Protected dashboard         | Yes           |
| `/sessions`                      | Session management          | Yes           |
| `/sessions/validate-token`       | Validate token (API)        | Yes           |
| `/sessions/refresh-token`        | Refresh token (API)         | Yes           |
| `/organization/settings`          | Protected settings page     | Yes (permission) |
| `/logout`                        | Logout and end session      | Yes           |

### 🚦 Try the app

1. Start the app (see Quick start)
2. Visit `http://localhost:3000`
3. Click Sign in with Scalekit
4. Authenticate with your provider
5. Open the dashboard and then try logout

Stuck? [Contact us](https://docs.scalekit.com/support/contact-us/).

#### Code structure

```
nextjs-scalekit-example/
├── app/                          # Next.js App Router pages
│   ├── api/                      # API routes
│   │   └── auth/                 # Authentication API routes
│   │       ├── login/            # Login initiation
│   │       ├── callback/          # OAuth callback handler
│   │       ├── logout/           # Logout handler
│   │       ├── refresh/          # Token refresh
│   │       └── validate/         # Token validation
│   ├── dashboard/                # Dashboard page
│   ├── sessions/                 # Session management page
│   ├── organization/             # Organization settings
│   ├── login/                    # Login page
│   ├── error/                     # Error page
│   └── permission-denied/        # Permission denied page
├── components/                    # React components
│   ├── LoginButton.tsx
│   ├── LogoutButton.tsx
│   └── SessionActions.tsx
├── lib/                          # Utility libraries
│   ├── scalekit.ts               # Scalekit client initialization
│   ├── cookies.ts                # Session cookie management
│   └── auth.ts                   # Authentication utilities
├── package.json                  # Dependencies
├── tsconfig.json                 # TypeScript configuration
├── next.config.js                # Next.js configuration
└── README.md                     # This file
```

#### Dependencies

- Next.js 14+ (App Router)
- **@scalekit-sdk/node** (Official Scalekit Node SDK)
- React 18+
- TypeScript
- date-fns (for date formatting)
- js-cookie (for cookie management)

See `package.json` for exact versions.

#### Scalekit SDK Methods Used

This application uses the official Scalekit Node SDK (`@scalekit-sdk/node`) for all authentication operations:

- `ScalekitClient.getAuthorizationUrl()` - Generate OAuth authorization URL
- `ScalekitClient.authenticateWithCode()` - Exchange code for tokens
- `ScalekitClient.validateToken()` - Validate tokens and extract permissions
- `ScalekitClient.refreshAccessToken()` - Refresh expired tokens
- `ScalekitClient.getLogoutUrl()` - Generate logout URL

#### Key Features

- **Server-Side Authentication**: Uses Next.js API routes for secure token handling
- **Cookie-Based Sessions**: Secure HTTP-only cookies for session storage
- **Automatic Token Refresh**: Middleware can be added to refresh tokens automatically
- **Permission-Based Access Control**: Route protection based on token permissions
- **TypeScript Support**: Full type safety throughout the application



#### Support

- Read the Scalekit docs: [Documentation](https://docs.scalekit.com).
- Read the Next.js docs: [Documentation](https://nextjs.org/docs).

#### License 📄

This project is for demonstration and learning. Refer to dependency licenses for production use.

