# Deviloq

**Language:** [العربية](./README.md) · English

**Build a portfolio that shows your work and your growth as a developer, then share it with one link.**

[Live website](https://deviloq.github.io/) · [Latest Android release](https://github.com/deviloq/deviloq.github.io/releases/latest)

![Deviloq preview](./social-preview.png)

## What is Deviloq?

Deviloq is a platform for creating a developer portfolio without editing code. Users create an account, add their work, skills, and learning progress, and get a public page they can share. It brings projects, achievements, learning, and a resume together instead of scattering them across separate links and files.

The project started as a personal portfolio named **MyDevFolioHub** and evolved into a multi-user platform called **Deviloq**. Its purpose remains the same: helping people show what they have built, what they are working on, and how they are growing over time.

## Why use it?

- **For students and developers:** bring projects, experiments, skills, education, certificates, and achievements into one profile.
- **To document learning:** show a learning log, current topics, goals, and resources alongside finished projects.
- **For applications and opportunities:** share a public portfolio and use the resume studio, with an A4 preview and readiness and ATS checklists.
- **To discover others:** browse public portfolios in the community and search or filter by specialty.

## Key features

- A dashboard for managing profiles and content, with a completion indicator and view analytics when data is available.
- Portfolio themes and section ordering, with a live preview while editing.
- Highlights of public GitHub repositories and programming languages when an account is connected.
- Public sections for projects, labs, learning, education, certificates, testimonials, and social links.
- Certificates can use a public HTTPS link or an uploaded file; their links appear in the portfolio and printable resume.
- Arabic and English interfaces with **RTL** support and layouts for desktop, tablet, and mobile.
- An installable **PWA** with an offline fallback page, plus an Android app built from the same web interface.

## How was it built?

Deviloq is a web app built with **HTML, CSS, and JavaScript**, without a heavy frontend framework. The frontend renders the pages and handles interactions, languages, and themes. **Supabase** provides authentication, data, and file storage, while **Brevo** delivers authentication email through the SMTP settings in Supabase. The app reads public GitHub data to highlight a user's work. Static web files are hosted on **GitHub Pages**, and **Capacitor** packages the same interface for Android.

| Part | Technology and role |
| --- | --- |
| Frontend | HTML, CSS, and JavaScript for the home, community, portfolio, and dashboard views |
| Accounts and data | Supabase Auth, Database, and Storage |
| Account email | Brevo SMTP sends email confirmation and password recovery messages through Supabase Auth |
| GitHub integration | GitHub API for public repositories and languages |
| Installable web app | Web Manifest and Service Worker for public app files and the offline fallback |
| Android | Capacitor, Gradle, and a signed APK for official releases |
| Verification | Playwright tests for public flows, mobile, Arabic, RTL, and horizontal overflow |

In short: **create an account → add content in the dashboard → save it to Supabase → share a public portfolio link**. The Android app uses the same frontend files, so design improvements can reach both web and mobile in a new release.

### Account email and redirects

**Supabase Auth** creates confirmation and password recovery messages and chooses the return URL; **Brevo** delivers those messages over SMTP. The site URL, allowed redirect URLs, and SMTP credentials are configured in the Supabase dashboard. Email passwords and API keys are not stored in this repository. After changing the website address, update the redirect URLs and any hard-coded links in email templates, then test an actual message. Sending volume depends on the Brevo plan and Supabase Auth rate limits.

## Run locally

You need **Node.js** and npm. The automated build workflow uses Node.js 24.

```bash
npm ci
node scripts/serve-site.cjs
```

Open `http://127.0.0.1:4173/`. To run the Playwright interface tests:

```bash
npm run test:e2e
```

You can preview the public interface locally, but account and content features need a connection to the project's Supabase service. This repository does not include the full database setup for an independent Supabase project; `sql/` contains a migration for resume content.

## Build the Android app

```bash
npm run mobile:sync
npm run mobile:apk
```

The first command copies the web files into the Android project. The second builds a debug APK. An **official APK that can update previous installations** needs the stable signing key and local configuration. See the [Android release guide](./ANDROID_RELEASE.md). Signing keys and passwords must not be committed to the repository.

## Project structure

```text
index.html          Website and app views
css/                Styles and fonts
js/                 Interface, account, and feature logic
assets/             Branding, fonts, and icons
sql/                Available database migrations
scripts/            Local server and Android asset preparation
tests/              Playwright tests
android/            Android app project
sw.js               Offline caching for public web files
```

## Notes

- Account content requires a network connection; the offline page does not provide offline editing or saving.
- Official Android releases use a stable signature and an increasing version code so they can update an installed app without uninstalling it.
- Licenses for bundled assets, including fonts and icons, are documented with those assets. See also the [HTML5 Boilerplate license](./LICENSE.txt).
