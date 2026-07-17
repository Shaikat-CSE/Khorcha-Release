<div align="center">
  <img src="public/logo.png" alt="Khorcha Logo" width="120" height="120" />
  <h1>Khorcha</h1>
  <p><strong>A beautifully engineered, iOS-native feeling expense tracker built with web technologies.</strong></p>
  
  <p>
    <a href="#features">Features</a> • 
    <a href="#tech-stack">Tech Stack</a> • 
    <a href="#running-locally">Running Locally</a> • 
    <a href="#building-for-android">Building for Android</a>
  </p>

  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailwind-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" />
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" />
  <img src="https://img.shields.io/badge/Capacitor-119EFF?style=for-the-badge&logo=capacitor&logoColor=white" />
</div>

---

## ✦ The Vision
Khorcha (খরচ - *Bengali for Expense*) is not just another budgeting app. It was designed from the ground up to feel like a **$100M fintech startup product**. By heavily utilizing raw CSS glassmorphism, fluid Framer Motion animations, and precise haptic feedback, Khorcha achieves an "Apple-level" premium aesthetic entirely within a web view.

> **Note:** The app is built as a Next.js Static Export (`out/`) which is then wrapped into a native Android APK using Iconic Capacitor.

---

## ✨ Features

- **💸 Effortless Tracking:** Instantly record income, expenses, and account transfers.
- **🏦 Vault & Accounts:** Manage liquid accounts (Bank, MFS) and track your Main Wallet with a live Total Liquidity dashboard.
- **🤝 Dedicated Debts Hub:** A powerful new dock tab specifically for liabilities. Organized into Borrow, Lend, and Bank Debts (Credit Cards + Bank Loans) with a comprehensive **Partial Settlement** system.
- **🎨 Thematic Branding:** Every interaction is dynamically colored to match its tab's icon (Blue, Orange, Pink, Purple, Green), creating a visually immersive navigation language.
- **🔒 Biometric Auth & PIN:** Secure your financial data with a gorgeous custom PIN pad, supporting native Biometrics (Fingerprint/FaceID via Capacitor).
- **🌐 Bilingual Support:** Instant toggle between English and Bengali UI.
- **🧠 Smart Notification Engine:** Dynamically tracks your average spending to identify anomalies natively in any currency. Alerts you of unusually high daily spending, potential subscriptions, and large transfers.
- **🌙 True Dark Mode:** An incredibly deep, OLED-friendly dark mode that makes neon accents pop.
- **⚡ Offline-First (Optimistic UI):** The UI updates instantly and syncs quietly to Supabase in the background.

---

## 🛠️ Tech Stack

**Frontend Framework:**
* React 19 + Next.js 16 (App Router, Static Export)

**Styling & Animation:**
* Tailwind CSS v4 (with custom raw CSS for advanced GPU-accelerated blurs)
* Framer Motion (Fluid gestures, spring layouts, layout transitions)

**Backend & Auth:**
* Supabase (PostgreSQL, Row Level Security, Google OAuth)

**Native Shell:**
* Capacitor (Android App wrapping, Biometrics, Status Bar, Haptics)

---

## 🚀 Running Locally

### 1. Clone & Install
```bash
git clone https://github.com/yourusername/KhorchaApp.git
cd KhorchaApp
npm install
```

### 2. Environment Variables
Create a `.env.local` file in the root directory and add your Supabase credentials:
```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
NEXT_PUBLIC_KHORCHA_UPDATE_MANIFEST_URL=https://your-saas-domain.com/api/khorcha/latest-version
NEXT_PUBLIC_KHORCHA_VERSION_CODE=4
```
*(See `.env.local.example` for reference).*

### In-App Update Notice

Host a public JSON response on your SaaS domain. The app fetches it after the user enters Khorcha and shows an update notice only when `versionCode` is greater than the installed Android build.

```json
{
  "versionCode": 4,
  "versionName": "1.0.4",
  "required": false,
  "title": "Update available",
  "message": "New features and fixes are ready.",
  "updateUrl": "https://your-saas-domain.com/khorcha/update"
}
```

Set `NEXT_PUBLIC_KHORCHA_UPDATE_MANIFEST_URL` before `npm run sync:android`. `updateUrl` must be HTTPS and should lead to your SaaS advertisement/download page. For each APK release, increase `versionCode` in `android/app/build.gradle`, publish the new APK, then increase `versionCode` in the manifest.

### Feedback API

Set `NEXT_PUBLIC_KHORCHA_FEEDBACK_URL` to a public HTTPS SaaS endpoint before building Android. Khorcha sends a JSON `POST` request when a user submits the Support & Dev feedback form:

```json
{
  "category": "bug",
  "message": "Transfer balance is wrong after editing.",
  "email": "user@example.com",
  "appVersion": "1.0.4",
  "buildCode": 4,
  "platform": "android"
}
```

Your endpoint must return any successful `2xx` response after storing feedback. Validate category and message server-side, limit messages to 2,000 characters, rate-limit requests, and return generic client-safe errors. Never expose a database service-role key in this app.

### 3. Run the Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) with your browser to see the app. 
> *Tip: For the best experience during web dev, test this using Chrome's Mobile Emulator.*

---

## 📱 Building for Android

Because Khorcha uses `next export`, we compile the static web files and feed them into Capacitor.

1. **Build the Next.js App**
   ```bash
   npm run build
   ```
   *(This generates the `/out` directory).*

2. **Sync with Capacitor**
   ```bash
   npx cap sync android
   ```

3. **Open Android Studio to Build the APK**
   ```bash
   npx cap open android
   ```
   From here, you can hit "Run" to test on an emulator, or go to `Build > Generate Signed Bundle / APK` to create your production release.

---

## 🎨 UI Philosophy
Khorcha heavily avoids "flat" design. Instead it relies on:
1. **Glassmorphism:** `backdrop-filter: blur(24px) saturate(120%)` creates a sense of depth and hierarchy.
2. **Ambient Orbs:** Soft CSS radial gradients in the background give frosted elements a color bleed to interact with.
3. **Typography Mastery:** SF Pro / Inter fonts with heavy tracking adjustments on small caps to mimic iOS native labels perfectly.
4. **Physicality:** Every interaction has `whileTap={{ scale: 0.94 }}` and cubic-bezier easing to mimic a physical spring mechanism. 

---

<p align="center">
  Built with ❤️ and ☕ by Shaikat S.
</p>
