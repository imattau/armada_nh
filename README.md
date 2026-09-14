# armada_nh

Native NostrHost (`_nh`) package for [Armada](https://armada.buzz)
([source](https://gitlab.com/soapbox-pub/armada)), an end-to-end encrypted
Nostr community chat SPA. Scriptless — see
[docs/security-model.md](docs/security-model.md) for why `[source.main]`
points at a pre-built artifact instead of building at install time.

Built from [imattau/nh-package-template](https://github.com/imattau/nh-package-template);
see that repo's `docs/new-package.md` for the general shape and
[docs/new-package.md](docs/new-package.md) here for anything armada-specific.

Companion classic package: [armada_ynh](https://github.com/imattau/armada_ynh)
(builds with `npm run build` at install time and lets the admin type
`app_relays`/`search_relays`/`platform_relays` at install).

**Behavior difference from armada_ynh:** this package has no install-time
questions, so `VITE_APP_NAME`/`VITE_APP_RELAYS`/`VITE_SEARCH_RELAYS`/
`VITE_PLATFORM_RELAYS` are fixed at build time instead (see `build.yml`'s
`BUILD_ENV`, currently armada_ynh's own hardcoded defaults). Every install of
a given package version gets the same relay configuration.

## Bumping to a new Armada version

Upstream doesn't reliably tag releases — armada_ynh itself pins a commit
hash, not a tag (see its `manifest.toml`).

1. Actions → "Build and publish artifact" → Run workflow, `upstream_ref` =
   the new pinned commit hash (or tag, if upstream cuts one).
2. Copy the printed release URL + SHA-256 from the job summary into
   `package.toml`'s `[source.main]`; bump `[app].version` to match.
3. Open a PR — `security.yml` validates the manifest and re-verifies the
   artifact hash.
