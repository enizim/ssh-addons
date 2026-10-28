# SSH Smart Home Dashboard — Add-on

Runs the dashboard inside Home Assistant, so it starts with Home Assistant, survives
reboots, and needs no Long-Lived Access Token.

---

## Installing

The add-on is built from source on the Home Assistant machine itself.

### 1. Package it

On the development machine, from the repo root:

```bash
npm run addon:package
```

This creates `dist-addon/ssh_dashboard/` — the add-on metadata plus a clean copy of
the source. `node_modules`, the local database and `.env` are excluded.

### 2. Copy it to Home Assistant

Copy the whole `ssh_dashboard` folder into Home Assistant's `addons` share, so it
ends up as `/addons/ssh_dashboard`.

Over the network share:

```
\\homeassistant\addons\ssh_dashboard
```

If the share isn't enabled, install the **Samba share** or **Advanced SSH & Web
Terminal** add-on first.

### 3. Install it

In Home Assistant: **Settings → Add-ons → Add-on Store → ⋮ → Check for updates**,
then reload the page. *SSH Smart Home Dashboard* appears under **Local add-ons**.

Click **Install**. The first build takes several minutes — it compiles the dashboard
inside the container. Watch progress in the add-on's **Log** tab.

### 4. Start it

Turn on **Start on boot** and **Watchdog**, then **Start**.

Open `http://homeassistant.local:8099` (or the Home Assistant machine's IP address on
port 8099). The first visit asks you to create the administrator account.

---

## Configuration

| Option | Values | Meaning |
|---|---|---|
| `log_level` | `debug`, `info`, `warning`, `error` | Verbosity in the add-on log. |
| `trust_proxy` | `0`–`5` | Number of reverse proxies in front. Set to `1` behind a Cloudflare Tunnel; leave at `0` for direct access. |

`trust_proxy` matters more than it looks. At `0` the add-on ignores `X-Forwarded-*`
headers, which is right when nothing is in front — otherwise any caller could forge
its own IP address and claim a plain-HTTP request arrived over HTTPS. Set to `1` and
the add-on believes the tunnel, which is what lets it mark the session cookie
`Secure` and log real client addresses.

The port is under the add-on's **Network** panel if 8099 clashes with something.

There is nothing else to configure. In particular there is **no Home Assistant URL or
token setting** — see below.

---

## How it connects to Home Assistant

The add-on declares `homeassistant_api: true`, so the Supervisor injects a
`SUPERVISOR_TOKEN` and exposes Home Assistant at `http://supervisor/core`. The server
uses that automatically.

This is a real improvement over the standalone setup:

- **No Long-Lived Access Token to create, paste, or leak.** The Supervisor issues the
  credential, and it stops working the moment the add-on stops.
- **Nothing to update when the Home Assistant IP changes.** `supervisor` is an
  internal name on the Docker network, so DHCP can move the host freely.

`SSH_HA_URL` + `SSH_HA_TOKEN` still work if they are set, and take priority — that
path is what the development setup uses, and it also covers pointing an add-on
install at a different Home Assistant instance.

---

## Why a port instead of Ingress

Ingress puts an add-on inside the Home Assistant sidebar, which looks appealing. It
is the wrong fit here, for two reasons.

**The dashboard has its own accounts.** Signing in with a name and a 4-digit PIN is
what the wall tablet is built around, and per-room permissions hang off those
accounts. Ingress requires a Home Assistant login first, so every family member would
need a Home Assistant account *as well as* an SSH one — two logins to reach one
screen, and two places where permissions are defined.

**A wall tablet is not a browser tab.** It should boot straight into the dashboard in
kiosk mode. A direct URL does that; an Ingress URL contains a session-scoped token
and can't be bookmarked.

Ingress can be added later as an *additional* way in without removing the port. It
needs the dashboard to work under a path prefix rather than at the site root, which
is a change to how the client builds API URLs — not a large change, but not free.

---

## Remote access

The add-on is reachable on the local network out of the box. Before exposing it to
the internet, three things need doing — the dashboard's own accounts are the only
thing between a stranger and the house.

1. **Never port-forward 8099 directly.** Use a Cloudflare Tunnel (no inbound ports at
   all, and the connector dials out) or a VPN.
2. **Put an identity layer in front.** Cloudflare Access adds an email or SSO check
   before anyone reaches the sign-in screen, so the PIN is never the only barrier.
3. **Tighten sign-in** under Settings → Sign-in security:
   - Turn **off** "Show account names on the sign-in screen" — otherwise the account
     list is handed to anyone who loads the page.
   - Raise **PIN length** to 6 or more. Existing accounts keep their current shorter
     PINs and keep working, so set a new PIN for each account afterwards or nothing
     actually changes.

Then set `trust_proxy` to `1` so the session cookie is marked `Secure`.

Repeated wrong PINs lock an account for 5 minutes, doubling to an hour — roughly
5 guesses an hour once it settles.

### Letting the mobile apps in

Cloudflare Access authenticates **browsers**: it redirects to an identity provider and
sets a cookie. A native app can't complete that, so it needs `/api/*` to bypass
Access — and the apps don't need it, because they pair once and then carry a 256-bit
device token instead of a PIN.

The catch is that PIN sign-in also lives under `/api/`. Bypassing it wholesale would
put a 6-digit PIN on the open internet with only the lockout in front. So:

1. In Cloudflare Zero Trust → **Access → Applications**, add a **second** application
   for the same hostname with the path `/api`, and give it a single **Bypass —
   Everyone** policy. The existing application keeps protecting everything else.
2. In the add-on's Configuration, set:

   ```yaml
   require_access_for_login: true
   access_team_domain: yourteam.cloudflareaccess.com
   access_aud: "<the application's Audience tag>"   # optional but recommended
   ```

   **The team domain is required.** The add-on verifies the Cloudflare identity
   token's *signature* against that team's published keys. Checking only that the
   header is present is not enough: on a Bypass path Cloudflare neither sets nor
   strips `Cf-Access-Jwt-Assertion`, so a client can simply send that header itself.
   With the guard on and no team domain set, PIN sign-in is refused rather than
   waved through.

   `access_aud` is the Application Audience tag from the Access application's
   overview page. Setting it scopes accepted tokens to this one application rather
   than any application in the team.

With that, requests to `/api/auth/login`, `/api/auth/setup` and `/api/auth/profiles`
are refused unless Cloudflare Access stamped them, while everything the apps use —
pairing, `/api/auth/me`, rooms, states, service calls — works with just a device
token. Browsers are unaffected: they arrive through Access and are stamped.

Worth adding alongside: a **Rate limiting** rule on `/api/` (Security → WAF). Bypassing
Access means bots now reach the origin, and that rule replaces what Access was
absorbing.

The guard applies **only to requests that came through Cloudflare**. Reaching the
add-on directly on the local network — the wall tablet, a browser at home — still
signs in with a PIN as normal. Without that exemption the household would be locked
out of its own dashboard, since a direct request never carries an Access header.

---

## Data and backups

Everything the add-on stores lives in `/data/dashboard.db`: users and PIN hashes,
rooms, device mappings, per-user preferences, the watch list, and activity history.

`/data` is the add-on's persistent volume. It survives restarts, add-on updates and
host reboots, and **Home Assistant's own backups include it** — so a full backup
captures the dashboard configuration too, with no separate export step.

The dashboard's own **Backup & Restore** page still works and produces a portable
JSON file, which is the one to use for moving configuration between installations.

---

## Troubleshooting

**The add-on doesn't appear in the store.** The folder must be directly under
`addons` — `/addons/ssh_dashboard/config.yaml`, not nested one level deeper. Then
**⋮ → Check for updates** and reload the page.

**The build fails.** Read the **Log** tab. A TypeScript error fails the build on
purpose, rather than shipping a broken dashboard.

**The build fails with `npm: not found`.** The Dockerfile is being built on the Home
Assistant base image, which has no Node in it. The Supervisor passes
`--build-arg BUILD_FROM=<HA base image>`, and that overrides both the `ARG` default in
the Dockerfile and anything in a `build.yaml`. The Dockerfile therefore ignores
`BUILD_FROM` and pins its own base in `ARG NODE_IMAGE`. If this error appears, an
older copy of the add-on is still on the host — delete `/addons/ssh_dashboard`
entirely, re-copy, and **Check for updates** again.

**A stale build keeps being reused.** Docker layer-caches aggressively. Uninstall the
add-on, delete the folder from the `addons` share, re-copy, then reinstall.

**Changes to the code don't show up after re-copying.** The Supervisor decides an
add-on has changed by looking at `version` in `config.yaml`, not at the files. Bump it
on every redeploy, then **⋮ → Check for updates** and use the **Update** button. To
confirm which build is actually running, compare `version` against the add-on page.

**It starts but shows no devices.** Check the log for the connection line:

```
[ha-bridge] using Supervisor Home Assistant at http://supervisor/core
[ha-bridge] connected to Home Assistant
```

If the first line is missing, `homeassistant_api` isn't in effect — reinstall the
add-on. If it connects but the dashboard is empty, the signed-in user may simply have
no rooms assigned; check with the administrator account.

**Cameras don't load.** Camera imagery is proxied through this add-on to Home
Assistant. Confirm the cameras work in Home Assistant itself first.

**The full-screen live view is black (snapshots are fine).** The Supervisor's API
proxy buffers responses rather than forwarding them, so an endless MJPEG stream never
arrives through it. The add-on works around this by asking Home Assistant to sign a
short-lived URL and fetching Home Assistant directly, falling back to a stream built
from snapshots. The log says which route is in use:

```
[camera] camera.x streaming via signed direct
[camera] camera.x streaming via snapshots
```

`via snapshots` means path signing wasn't available; the view works but is choppier,
roughly 1–2 frames per second.
