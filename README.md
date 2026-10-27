# Wealthfolio — Unraid Community Apps

Unraid Community Apps (CA) submission for [**Wealthfolio**](https://wealthfolio.app),
a beautiful, open-source, local-first personal finance tracker.

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
   - `WF_AUTH_PASSWORD_HASH` — `printf '%s' 'your-password' | argon2 yoursalt16chars -id -e`
   - `WF_CORS_ALLOW_ORIGINS` — the exact URL you'll use to reach the app
     (e.g. `http://192.168.1.10:8088`)
4. Click **Apply**.

Full setup guide, reverse-proxy notes, backup, and troubleshooting:
<https://wealthfolio.app/docs/guide/self-hosting/unraid>

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

Then **Docker → Add Container → Template → User templates → Wealthfolio**.

## Optional OIDC private CA

For an identity provider using a private CA, use a server build containing
[wealthfolio/wealthfolio#2253](https://github.com/wealthfolio/wealthfolio/pull/2253).
Upgrade older pinned images before enabling this setting. Existing installs and
public-CA identity providers can leave `WF_OIDC_CA_FILE` empty; the template
adds no certificate mount by default.

In **Docker → wealthfolio → Edit**:

1. Add a **Path** mapping from an existing host PEM CA bundle to
   `/certs/internal-ca.pem`, with access mode **Read Only**.
2. Set the advanced `WF_OIDC_CA_FILE` variable to `/certs/internal-ca.pem`. If
   an older installed template does not show it, add it as a **Variable**.
3. Ensure the bundle is readable by the actual container user (`99:100` with
   this template). Keep the file and its parent directory
   administrator-controlled, then click **Apply** to recreate the container.

The bundle contains CA certificates, not private keys. It adds trust only to
OIDC requests, keeps the bundled public CAs, and preserves certificate and
hostname validation. Browsers must separately trust the IdP certificate. Restart
the container after replacing the bundle; it is loaded once at startup. An
unreadable file, invalid certificate, or bundle without certificates prevents
startup when OIDC is enabled.

## Support

- **Forum thread:** <https://forums.unraid.net/topic/198598-afadil-wealthfolio/>
- **Bug reports:** <https://github.com/wealthfolio/wealthfolio/issues>

## License

The template and metadata in this repository are released under the
[MIT License](./LICENSE). The Wealthfolio application itself is licensed under
[AGPL-3.0](https://github.com/wealthfolio/wealthfolio/blob/main/LICENSE).
