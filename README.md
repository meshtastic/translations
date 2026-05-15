# Meshtastic Translations

Community-powered translations of Meshtastic app documentation, contributed automatically by users around the world.

## How It Works

The Meshtastic Apple app ships with a complete set of English documentation. When a user opens Help & Docs in a non-English language, the app translates every page on-device using Apple's Translation framework (or the on-device language model on iOS 26+).

**Here's the magic:** the very first person to use a particular language + app version combination automatically contributes their translations back to this repository. Every user after them gets those translations instantly from our CDN.

### The Translation Loop

```
1. 🇫🇷 Marie opens Help & Docs in French on v2.7.13
2. 📱 No community translations exist yet — her device translates all pages locally
3. ☁️ After translation completes, her app uploads the translated pages here
4. 📋 A manifest.json, nav-labels.json, and search-index.json are committed alongside
5. 🇫🇷 The next French user on v2.7.13 gets Marie's translations instantly from CDN
6. 🔁 The cycle repeats for every new language and app version
```

No sign-up, no manual work, no copy-pasting. It just happens in the background while you use the app.

### What Gets Uploaded

For each language + app version, the app commits:

| File | Purpose |
|------|---------|
| `user/*.md` | Translated user guide pages (markdown) |
| `developer/*.md` | Translated developer guide pages (markdown) |
| `manifest.json` | Completeness marker — lists all pages, version, timestamp |
| `nav-labels.json` | Translated page titles and section names for the sidebar |
| `search-index.json` | Translated keywords so users can search docs in their language |

### Opting Out

Users can disable this by toggling off **"Participate in Distributed Translations"** in App Settings. The toggle is on by default. Translations are uploaded anonymously — no user data is included.

## Repository Structure

```
apple-apps/
  {language}/
    {app-version}/
      manifest.json
      nav-labels.json
      search-index.json
      user/
        messages.md
        nodes.md
        map.md
        ...
      developer/
        architecture.md
        testing.md
        ...
```

- **`{language}`** — ISO 639-1 language code (e.g., `fr`, `de`, `es`, `ja`)
- **`{app-version}`** — The app version that shipped the English source docs (e.g., `2.7.13`)

## For Native Speakers

These translations are machine-generated on-device. They're a great starting point, but they're not perfect! If you're a native speaker and spot something that could be improved:

1. Browse to the language + version you want to fix
2. Edit the `.md` file directly on GitHub
3. Submit a pull request

Your improvements will be served to every user of that language going forward.

## How the App Fetches Translations

The app checks an `index.json` feed (served via GitHub Pages) on launch. If translations exist for the user's language and app version, they're downloaded directly — skipping on-device translation entirely. This means:

- **First user of a language/version:** ~30 seconds of on-device translation, then auto-upload
- **Every user after that:** Instant translated docs

## License

Translations are contributed under the same license as the [Meshtastic Apple app](https://github.com/meshtastic/Meshtastic-Apple) (GPL-3.0).
