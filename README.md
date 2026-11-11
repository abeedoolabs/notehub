# notehub

An internal pastebin. Publish an HTML or Markdown page, get a link, send the link.

Notes start **private** and widen only when you say so — to anyone signed in to your
organisation, to the organisation directory, or to a real public web page with no account
needed. HTML renders exactly as written, scripts and all, inside a sandboxed frame that
cannot reach notehub or the page around it.

**Hosted instance and full documentation: <https://notehub.radishplatform.com>**

---

## Install

The container image is public. Everything below pulls
`ghcr.io/abeedoolabs/notehub:latest` and needs no credentials.

### Coolify — one paste

**New Resource → Docker Compose Empty**, paste
<https://notehub.radishplatform.com/install/coolify.yaml>, **Deploy**.

Coolify issues the domain, gets the certificate, generates both secrets and wires the
app's own address through. There is nothing to fill in afterwards.

### Docker Compose — anywhere

```bash
curl -o docker-compose.yml https://notehub.radishplatform.com/install/docker-compose.yml

cat > .env <<ENV
SESSION_SECRET=$(openssl rand -hex 32)
RADISH_ENCRYPTION_KEYS=k1:$(openssl rand -hex 32)
ENV

docker compose up -d
```

Open `http://localhost:3000`. The first page creates the administrator account, asks
whether this install is one organisation or several, and offers to publish a landing
page. Then you are done.

### Keep that `.env`

Compose refuses to start without both secrets rather than starting with empty ones.

- **`SESSION_SECRET`** signs session cookies. Lose it and everyone is signed out.
- **`RADISH_ENCRYPTION_KEYS`** decrypts stored API keys. Lose it and every issued key
  stops working, permanently.

Back it up somewhere other than the server it runs on.

### Architecture

**amd64 only.** On an arm64 machine Docker reports an exec-format error, which looks
like a corrupt image and is not.

---

## Publishing from Claude Code

A skill that publishes a file to your notehub and hands back the link:

```bash
curl -L https://notehub.radishplatform.com/skill/notehub-publish.tar.gz \
  | tar -xz -C ~/.claude/skills/
```

Then create an API key at **/profile/api-keys** on your instance and run the two commands
it shows you.

---

## Why this repository has no source in it

notehub's source is not published. This repository exists to carry install documentation
and to give the container package a home; the application itself is developed privately.

Nothing here is a copy of anything. The compose files and the skill are served by the
running application and are inlined into the image at build time, so the files an
instance hands out are necessarily the ones that instance was built from. They cannot
drift from the release, which a checked-in copy here would eventually do.

## Links

| | |
|---|---|
| Hosted instance | <https://notehub.radishplatform.com> |
| Container image | `ghcr.io/abeedoolabs/notehub` |
| Coolify template | <https://notehub.radishplatform.com/install/coolify.yaml> |
| Compose file | <https://notehub.radishplatform.com/install/docker-compose.yml> |
| Claude skill | <https://notehub.radishplatform.com/skill/notehub-publish.tar.gz> |
