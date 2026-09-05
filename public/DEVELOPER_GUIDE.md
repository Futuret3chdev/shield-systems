# Futuret3ch Security — Shield Systems  
## Developer Guide (Public)

**Defensive visibility only · No watermarks · Aligned with live site**

Part of the [MemeTorrent](https://memetorrent.futuret3ch.com.au) ecosystem by [Futuret3ch](https://futuret3ch.com.au).

| Resource | URL |
|----------|-----|
| Shield product surface | https://memetorrent.futuret3ch.com.au/shield |
| **Shield Product API** | https://memetorrent.futuret3ch.com.au/shield/api |
| **Developers hub** | https://memetorrent.futuret3ch.com.au/developers |
| MT Games SDK | https://memetorrent.futuret3ch.com.au/software/games/sdk |
| Studio Suite (incl. Shield) | https://memetorrent.futuret3ch.com.au/software/games |
| Site policies | https://www.futuret3ch.com.au/policies.html |
| Support | support@futuret3ch.com.au |

---

## 1. Where Shield sits

Shield is listed in the **MT Studio Suite** alongside Android compiler, Device Lab, Publisher, Play Console, iOS, Photo-to-game, World 3D, Music, Video, and Bot.

On the public site it appears as:

- Top navigation item **Shield**
- Product API at `/shield/api`
- Control path intended at `memetorrent.futuret3ch.com.au/shield`

Shield Systems (UEIS) is the defensive-visibility layer: Endpoint, Edge, and Data shields. It is designed to help operators understand what is stored, running, and communicating — **not** to decrypt, unlock, or bypass platform protections.

---

## 2. Developers hub (site-wide)

**URL:** https://memetorrent.futuret3ch.com.au/developers

Public surfaces include:

| Area | Purpose |
|------|---------|
| **Market data** | Keyless CMC-style quotes, listings, OHLCV |
| **TAP** | Trips, packages, food · TAPSHOP · TAPMATCH (not games) |
| **Token tracker** | Mint, pool, holders, chart-style paths |
| **Play SDK** | Catalog games · wallets · portal login · scores |
| **CLI** | `node mt.js` / PowerShell installer for quotes, TAP, holders, chart, scores |
| **Studio** | $MT shop, editor, titles API |
| **Wallet & MT-Chain** | Preview routes; same host when live |

### Quickstart (from live page)

```bash
curl "https://memetorrent.futuret3ch.com.au/api/v1/cryptocurrency/quotes/latest?symbol=MT"
```

**Auth notes (public):** Market, tracker, and TAP *reads* are keyless. TAP writes, portal, studio, chat, and claims use the site session cookie. Header `X-MT-API-KEY` is reserved for paid / chain launch.

---

## 3. Shield Product API (live routes)

**URL:** https://memetorrent.futuret3ch.com.au/shield/api

Site-wide Developers API (MT-Connect, wallets, social login) lives at `/developers`. The Shield page lists **Shield product routes** only.

**Auth:** user Bearer token or Head Office key.  
Billing checkout is not fully live — PayID now; cards and further billing coming soon.

### Auth & profile

| Method | Path | Notes |
|--------|------|--------|
| POST | `/v1/auth/signup` | Email + password · returns session |
| POST | `/v1/auth/login` | Email + password |
| POST | `/v1/auth/oauth/google` | Google sign-in · avatar for profile |
| POST | `/v1/auth/oauth/facebook` | Facebook sign-in · avatar for profile |
| GET | `/v1/me` | Profile, plan, trial clock, add-ons, threat level |
| PATCH | `/v1/me/threat-level` | `everyday` \| `elevated` \| `stalking` \| `child` |

### Devices & scans

| Method | Path | Notes |
|--------|------|--------|
| GET | `/v1/me/devices` | Licensed devices for this account |
| POST | `/v1/devices/:id/heartbeat` | OS, Shield version, last scan |
| POST | `/v1/devices/:id/diagnostics` | **User-approved upload only** |
| GET | `/v1/scans` | Scan history · real results only |
| POST | `/v1/scans` | Start on-demand scan on a licensed device |

### Guide, family, support, add-ons

| Method | Path | Notes |
|--------|------|--------|
| POST | `/v1/guide/chat` | Guide AI · threat-level playbooks · **refuses attack advice** |
| POST | `/v1/family/link` | Create visible guardian link code |
| POST | `/v1/family/accept` | Child accepts · banner always shown · child can unlink |
| GET | `/v1/family/children/:id/presence` | Last seen · location only if Location Share is on |
| POST | `/v1/support/sessions` | User creates · staff join only after Allow |
| GET | `/v1/addons` | Realtime AV, Network Ops, Family Link, Breach Watch, Concierge |

Always treat the live `/shield/api` page as the source of truth for paths and behaviour.

---

## 4. MT Games SDK (related public API)

**URL:** https://memetorrent.futuret3ch.com.au/software/games/sdk

```html
<script src="https://memetorrent.futuret3ch.com.au/sdk/mt-games.js"></script>
```

```javascript
const games = MTGames.init({ gameId: 'my-title' });
await games.verify('MT-FREE-…');   // license from portal / Software → Developers
games.postScore(900, { room: games.partyCode() });
games.scores({ limit: 10 });
```

| Call | Backend |
|------|---------|
| `verify(key)` | `GET /api/v1/games/license` |
| `me()` | Portal session + license on profile |
| `postScore` / `scores` | Same boards as Play |
| `partyCode()` | Four-letter room |
| `apkUrl()` | Android MT Games APK |

Builder license is issued on portal sign-in (free tier). Pro upgrade is announced as coming soon on the site.

---

## 5. Design principles for integrators

1. **Visibility ≠ bypass** — Report integrity, entropy anomalies, persistence, and process-level network accounting. Do not decrypt content, unlock credentials, or break sandboxes (SIP, TCC, Scoped Storage, etc.).
2. **User / operator consent** — Diagnostics and support sessions require explicit allow. Family Link is visible and revocable by the child.
3. **Guide AI refuses attack advice** — The public Guide endpoint is documented to refuse attack guidance; keep the same boundary in any client you build.
4. **Real results only** — Scan history endpoints are described as returning real results, nothing invented. Match that standard.
5. **Align with site policies** — Privacy and terms live at https://www.futuret3ch.com.au/policies.html (Australian Privacy Principles). Mobile app sections already cover limited device telemetry and permission prompts.

---

## 6. Suggested engagement path

1. Create / sign in via portal (Users · Developers · Businesses).  
2. Obtain builder license if shipping under Studio / Games.  
3. Read live API pages: `/developers` and `/shield/api`.  
4. For Shield product work, use Bearer or Head Office key as documented.  
5. For market/TAP/Play, follow the keyless or session rules on the Developers page.  
6. Questions: support@futuret3ch.com.au  

---

## 7. Boundaries (public statement)

Shield Systems and related Futuret3ch Security materials are for **defensive visibility**:

- No decryption or unlocking of protected content  
- No bypass of passwords, encryption, or platform sandboxes  
- No tools for unauthorised access to other people’s data  
- Use must comply with law, policy, and platform terms  

Detailed engineering specifications remain internal to Futuret3ch Security.

---

© Futuret3ch Security · MemeTorrent ecosystem  
Documents in this pack have **no watermarks**.
