# ShadowMint v3.0 — Project Handoff
_Paste this into a new Claude chat to continue exactly where we left off._

---

## 🧠 CONTEXT FOR CLAUDE

Hi Claude! I'm building **ShadowMint v3.0**, a React + TypeScript fintech wallet app. Here's exactly where we are:

---

## 📁 PROJECT

- **Stack:** React 18 + TypeScript + Vite 5.4.21, Zustand, React Router 6, Supabase, Capacitor
- **Backend:** Express 5.2.1 + Node 20 (server/ folder — not used in production, Supabase handles everything)
- **Live URL:** https://shadowmint-eight.vercel.app
- **GitHub:** https://github.com/MintShadow/shadowmint
- **Environment:** Development in StackBlitz WebContainers (jsh shell — use Node REPL to write files)

---

## ⚠️ IMPORTANT: STACKBLITZ ENCODING

- Use plain ASCII, no unicode dashes `--` `---`, use `-` instead
- No curly quotes, use straight quotes `"` only
- Use `&middot;` or ` · ` for separators
- In jsh: NO bash redirects, NO `printf`, use Node REPL to write files:

```bash
node
> var fs = require('fs')
> fs.writeFileSync('/path/to/file', 'content')
> .exit
```

---

## 🔧 SUPABASE CONFIG

- **URL:** `https://isdiomwcqcfefebzudpd.supabase.co`
- **Anon key:** `sb_publishable_YequU0Twarq7cLeTYalyxw_T87ggxSS`
- **Auth redirect:** `https://shadowmint-eight.vercel.app/**`

### Tables

- `profiles` — auto-created on signup, has `full_name`, `username`, `avatar_url`, `phone`, `city`, `country`
- `wallets` — auto-created on signup, `balance` in **cents**
- `transactions` — `type: credit/debit`, `category: transfer/deposit/withdrawal/request/fee/refund`
- `bank_accounts` — saved BSB + account numbers
- `kyc_status` — auto-created on signup, `status: unverified/in_progress/pending/verified/failed`
- `payment_requests` — `requester_id`, `recipient_id`, `amount` (cents), `note`, `status: pending/paid/declined`, `expires_at`

### RPC Function

```sql
CREATE OR REPLACE FUNCTION public.get_user_id_by_email(email TEXT)
RETURNS TABLE(id UUID)
LANGUAGE sql SECURITY DEFINER
AS $$
  SELECT id FROM auth.users WHERE auth.users.email = $1 LIMIT 1;
$$;
```

---

## 🎨 COLOR TOKENS

```
Mint accent:     #35f2a8
Mint gradient:   linear-gradient(135deg, #35f2a8 0%, #18c87a 100%)
Deep green:      #0d7a5f
Balance card:    linear-gradient(135deg, #065f46 0%, #0d7a5f 40%, #10b981 100%)
Background:      #060810
Card bg:         rgba(255,255,255,0.04)
Card border:     rgba(255,255,255,0.08)
Text primary:    #eef0f8
Text dim:        rgba(238,240,248,0.45)
Text muted:      rgba(238,240,248,0.28)
Danger:          #f87171
Warning:         #f6a623
Info:            #60a5fa
Button text:     #050c18 (on mint buttons)
```

---

## ✅ ALL COMPLETED FEATURES

### Auth
- `src/screens/Login.tsx` ✅
- `src/screens/SignUp.tsx` ✅
- `src/pages/SplashScreen.tsx` ✅
- `src/components/ProtectedRoute.tsx` ✅

### Core
- `src/App.tsx` ✅
- `src/components/BottomNav.tsx` ✅ — 5 tabs with orange badge on Activity for pending requests, polls every 30s

### Wallet
- `src/pages/Wallet/WalletOverviewPage.tsx` ✅ — balance card with hide/show toggle, colour-coded action buttons, pending requests banner (dismissible), rich empty state with onboarding CTAs, greeting with avatar

### Send Money
- `src/pages/Send/SendMoneyPage.tsx` ✅
- `src/pages/Send/SendReviewPage.tsx` ✅
- `src/pages/Send/SendSuccessPage.tsx` ✅

### Request Money
- `src/pages/Request/RequestJust.tsx` ✅ — create request by email
- `src/pages/Request/IncomingRequestsPage.tsx` ✅ — `/requests` — list of all incoming requests, pending/all filter, inline Pay + Decline buttons
- `src/pages/Request/RequestReviewPage.tsx` ✅ — `/requests/:id` — full approval screen with balance check
- `src/pages/Request/RequestPaySuccessPage.tsx` ✅

### Guest Payment (no account needed)
- `src/pages/Guest/GuestPaymentPage.jsx` ✅ — `/guest/pay/:requestId` — loads real data from Supabase, card/PayID/bank transfer flows, loading/error/already-paid states

### Public Payment Page
- `src/pages/Public/PublicPaymentPage.tsx` ✅ — `/pay/:username` — works for logged-in and logged-out users

### Activity
- `src/pages/Activity/ActivityPage.tsx` ✅ — transactions + requests tabs, pending badge
- `src/pages/Activity/TransactionDetailsPage.tsx` ✅

### Deposit
- `src/pages/Deposit/BankDepositInstructionPage.tsx` ✅

### Withdraw
- `src/pages/Withdraw/WithdrawPage.tsx` ✅
- `src/pages/Withdraw/WithdrawConfirmPage.tsx` ✅
- `src/pages/Withdraw/WithdrawSuccessPage.tsx` ✅

### Profile
- `src/pages/Wallet/ProfilePage.tsx` ✅ — avatar, username, balance, KYC status, shareable payment link with copy + QR code, KYC nudge banner, settings menu
- `src/pages/Profile/EditProfilePage.tsx` ✅
- `src/pages/Profile/BankDetailsPage.tsx` ✅

### KYC (7 pages)
- `src/pages/Kyc/KycStartPage.tsx` ✅
- `src/pages/Kyc/KycDocumentTypePage.tsx` ✅
- `src/pages/Kyc/KycDocumentUploadPage.tsx` ✅ — uploads to Supabase storage `kyc-documents` bucket
- `src/pages/Kyc/KycSelfiePage.tsx` ✅ — uploads selfie to storage
- `src/pages/Kyc/KycPendingPage.tsx` ✅
- `src/pages/Kyc/KycSuccessPage.tsx` ✅
- `src/pages/Kyc/KycFailedPage.tsx` ✅

### Settings
- `src/components/Settings/ActiveSessionPage.tsx` ✅

---

## 🗺️ ROUTES

```
/                         -> SplashScreen
/login                    -> Login
/signup                   -> SignUp
/pay/:username            -> PublicPaymentPage (public)
/guest/pay/:requestId     -> GuestPaymentPage (public)
/wallet                   -> WalletOverviewPage (protected)
/send                     -> SendMoneyPage (protected)
/send/review              -> SendReviewPage (protected)
/send/success             -> SendSuccessPage (protected)
/request                  -> RequestJust (protected)
/requests                 -> IncomingRequestsPage (protected)
/requests/:id             -> RequestReviewPage (protected)
/request-success          -> RequestPaySuccessPage (protected)
/activity                 -> ActivityPage (protected)
/activity/:id             -> TransactionDetailsPage (protected)
/deposit/bank             -> BankDepositInstructionPage (protected)
/withdraw                 -> WithdrawPage (protected)
/withdraw/confirm         -> WithdrawConfirmPage (protected)
/withdraw/success         -> WithdrawSuccessPage (protected)
/profile                  -> ProfilePage (protected)
/profile/edit             -> EditProfilePage (protected)
/profile/bank-details     -> BankDetailsPage (protected)
/kyc                      -> KycStartPage (protected)
/kyc/document-type        -> KycDocumentTypePage (protected)
/kyc/upload               -> KycDocumentUploadPage (protected)
/kyc/selfie               -> KycSelfiePage (protected)
/kyc/pending              -> KycPendingPage (protected)
/kyc/success              -> KycSuccessPage (protected)
/kyc/failed               -> KycFailedPage (protected)
/settings/sessions        -> ActiveSessionsPage (protected)
```

---

## 🚀 DEPLOYMENT

- **Vercel:** https://shadowmint-eight.vercel.app (auto-deploys on push to `main`)
- **vercel.json** at root: `{"rewrites":[{"source":"/(.*)", "destination":"/index.html"}]}`
- **Env vars on Vercel:** `VITE_SUPABASE_URL` + `VITE_SUPABASE_ANON_KEY`
- **To deploy:** commit changes on Mac then `git push` and Vercel auto-builds

---

## 📋 POSSIBLE NEXT STEPS

1. **Custom domain** — connect `shadowmint.app` or similar via Vercel Settings -> Domains
2. **Push notifications** — notify users when they receive money or a request
3. **Native app** — Capacitor already set up with `android/` and `ios/` folders
4. **Admin dashboard** — approve/reject KYC, view all transactions
5. **Real bank integration** — connect to Monoova or Zepto for real AU payments
6. **Transaction limits** — daily/weekly send limits based on KYC status

---

_Updated: 2026-03-03 | ShadowMint v3.0 — Live on Vercel_ 🎉
