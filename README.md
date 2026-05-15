# Wealthfolio — Unraid Community Apps

Unraid Community Apps (CA) submission for [**Wealthfolio**](https://wealthfolio.app),
a beautiful, open-source, local-first investment and net worth tracker.

This repository hosts only the CA metadata. The application source, Docker image,
and self-hosting documentation live in the upstream repository:
[`wealthfolio/wealthfolio`](https://github.com/wealthfolio/wealthfolio).

## What's here

- `ca_profile.xml` — repository profile shown in Community Apps.
- `templates/wealthfolio.xml` — the Docker app template (image, ports, volumes,
  env vars). CA fetches it directly from this repo's `main` branch.

## Install (once the repo is approved by CA)

1. Open the **Apps** tab on Unraid.
2. Search for **Wealthfolio**.
3. Click **Install** and fill in the three required values:
   - `WF_SECRET_KEY` — `openssl rand -base64 32` (back this up!)
   - `WF_AUTH_PASSWORD_HASH` — `printf 'your-password' | argon2 yoursalt16chars! -id -e`
   - `WF_CORS_ALLOW_ORIGINS` — the exact URL you'll use to reach the app
     (e.g. `http://192.168.1.10:8088`)
4. Click **Apply**.

Full setup guide, reverse-proxy notes, backup, and troubleshooting:
<https://wealthfolio.app/docs/guide/self-hosting>

## Sideload before CA approval

If you want to install Wealthfolio on Unraid before this repository has been
merged into Community Apps, sideload the template directly. SSH into Unraid
(or use the WebTerminal) and run:

```bash
mkdir -p /boot/config/plugins/dockerMan/templates-user
curl -fsSL \
  https://raw.githubusercontent.com/wealthfolio/wealthfolio-unraid/main/templates/wealthfolio.xml \
  -o /boot/config/plugins/dockerMan/templates-user/my-wealthfolio.xml
```

Then **Docker → Add Container → Template → User templates → wealthfolio**.

## Support

- **Forum thread:** <https://forums.unraid.net/topic/198598-afadil-wealthfolio/>
- **Bug reports:** <https://github.com/wealthfolio/wealthfolio/issues>

## License

The template and metadata in this repository are released under the
[MIT License](./LICENSE). The Wealthfolio application itself is licensed under
[AGPL-3.0](https://github.com/wealthfolio/wealthfolio/blob/main/LICENSE).
