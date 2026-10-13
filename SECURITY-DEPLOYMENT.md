# FAWZ Firebase security and deployment notes

This build uses Firebase Authentication, server-verified session cookies, Firestore profiles, five server-selected price tiers, CSRF checks, strict request validation and audit events. Public catalogue HTML contains no live non-zero prices.

## Mandatory production controls

- Deploy the supplied deny-by-default "firestore.rules".
- Never place FIREBASE_PRIVATE_KEY or service-account JSON in public code.
- Restrict the Firebase web API key to required Firebase APIs and approved website origins in Google Cloud Console.
- Add final Netlify and custom domains under Firebase Authentication > Settings > Authorized domains.
- Keep Netlify environment variables scoped deliberately.
- Require strong unique temporary passwords and remove them after initialization.
- Keep at least two controlled administrator accounts for recovery.
- Review Firebase Authentication and Firestore usage regularly.
- Export/back up Firestore data and test restoration.
- Revoke user sessions immediately when access or tier changes.
- Keep Firebase Admin and other dependencies updated after testing.
- Re-run "npm test" before every deployment.

## Data model

- users/{uid}: email, displayName, tier A–E, role, approved, disabled, mustChangePassword.
- product_prices/{productId}: a tiers map containing A–E numeric prices.
- audit_log/{eventId}: administrative and authentication audit events.

Browser access to all Firestore documents is denied. Only the Netlify Functions service account can read or write protected documents.

