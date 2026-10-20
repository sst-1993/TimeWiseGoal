# TimeWiseGoal

**TimeWiseGoal** is a goal-planning and execution platform for individuals and teams, available as a web app, desktop app, and Flutter client. It helps users turn plans into actionable, trackable daily work through goal breakdown, task tracking, and AI-assisted creation.

> This repository includes multiple clients and server-side configurations. Feature coverage may vary by client. A complete setup requires your own Supabase project and any third-party services you use.

## Features

- **Goals and tasks**: Break goals into goal trees, create and track tasks, monitor progress, and set reminders.
- **AI Studio**: AI assistants, content generation, and multimodal creation. Available models depend on the server-side functions and API configuration.
- **Visual workflows**: Organize AI and multimedia processing steps with nodes, and inspect execution status and results.
- **Community**: Forums, community events, friends, and other social features.
- **Personal workspace**: Profile, notifications, themes, language settings, and more.
- **Multi-platform support**: React web app, Tauri desktop app, and Flutter mobile app. The Flutter project also includes configuration for web support.

Some optional modules depend on Supabase Edge Functions, database migrations, and third-party services. Cloning the repository does not automatically provide production capabilities.

## Tech Stack

| Area | Technologies |
| --- | --- |
| Web | React 18, TypeScript, Vite, Tailwind CSS |
| Desktop | Tauri 2, Rust |
| Mobile | Flutter, Dart, Provider, Material Design |
| Backend integration | Supabase Auth, PostgreSQL, Realtime, Storage, Edge Functions |

## Repository Structure

```text
.
├── src/                 # React web app
├── src-tauri/           # Tauri desktop shell and Rust code
├── flutter_app/         # Flutter client (Android, iOS, Web, etc.)
├── supabase/            # Edge Functions, database migrations, and related configuration
├── lib_flutter/         # Standalone Flutter analytics SDK
└── package.json         # Web / Tauri scripts and dependencies
```

## Getting Started: Web

### Prerequisites

- Node.js 20 LTS or later
- npm
- A Supabase project

### Install and Run

Install dependencies:

```bash
npm install
```

Copy the environment template to a local configuration file:

```powershell
Copy-Item .env.example .env.local
```

At minimum, set the following values in `.env.local`:

```dotenv
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

Start the development server:

```bash
npm run dev
```

Common commands:

```bash
npm run build     # Build the web app
npm run preview   # Preview the production build locally
npm run lint      # Run ESLint
```

## Flutter Client

You need the Flutter and Dart SDKs (the project requires Dart `>=3.0.0 <4.0.0`) and the toolchain for your target platform. Building for iOS requires macOS and Xcode.

```bash
cd flutter_app
flutter pub get
flutter run
```

To run on a specific platform:

```bash
flutter run -d chrome     # Web
flutter run -d android    # Android
flutter run -d ios        # iOS (macOS)
```

Example release builds:

```bash
flutter build web --release
flutter build apk --release
flutter build appbundle --release
flutter build ios --release   # macOS + Xcode
```

The Flutter Supabase URL and anon key are currently provided by [supabase_config.dart](flutter_app/lib/config/supabase_config.dart). Update this configuration to connect your own Supabase project. Never put a Supabase `service_role` key or other private credentials in client-side code.

## Backend and Third-Party Services

Both the web and Flutter clients depend on Supabase. A complete local setup requires more than the Supabase URL and anon key: enable the database tables, Row Level Security (RLS) policies, Storage buckets, and Edge Functions required by the features you plan to use. Related migrations and functions are located in `supabase/`. Review them and apply them to your own Supabase project using your deployment process. Do not use a production database for testing.

AI generation, payments, object storage, maps, and other integrations may require additional third-party accounts and credentials. Refer to `.env.example` and the documentation for each feature under `supabase/functions/`. Secrets with server-side privileges must be stored only on the server or as Supabase Edge Function secrets. Any value prefixed with `VITE_` is included in the frontend build and must not be treated as secret.

## Desktop Development

The desktop app is built with Tauri 2. In addition to Node.js/npm, you need the Rust toolchain and the Tauri dependencies for your operating system. Use the project scripts to start or build the app:

```bash
npm run tauri:dev
npm run tauri:build
```

The desktop scripts currently use Unix-like shell syntax. On Windows, configure Rust, WebView2, and the build environment according to the Tauri documentation, and adjust how you invoke the scripts as needed.

## Contributing

Use Issues to discuss problems or propose improvements. Before submitting a pull request, confirm the scope of your changes and describe what changed, how you verified it, and whether it requires database migrations or external service configuration. Database changes should include the relevant migrations and be reviewed for RLS and permission boundaries.

## Security

- Do not commit `.env.local`, access tokens, service-role keys, or production credentials.
- If a secret has ever been committed, revoke and rotate it with the relevant provider immediately—even if the file has since been deleted.
- Before deploying publicly, review database RLS, Edge Functions, CORS, payment callbacks, and Storage permissions.

## License

There is currently no root-level `LICENSE` file, so the project's open-source license has not been specified. Before publishing or accepting external contributions, maintainers should choose and add an appropriate license. Making the README or source code public does not by itself grant rights to copy, modify, or redistribute it.

