# hugo-starter

A template for a new Hugo site built on
[hugo-base](https://github.com/bradryanbice/hugo-base). Deliberately thin: its
job is the first ten minutes, and everything it does not contain comes from the
module.

A site generated from this template builds, passes the accessibility and
quality gate, and deploys, before you change anything.

## The first ten minutes

1. **Use this template** on GitHub, and clone your new repository.

2. **Install the toolchain.** Versions are pinned, and CI fails if they drift.

   ```sh
   mise install
   ```

3. **Change four values** in `config/_default/hugo.toml`: `baseURL`, `title`,
   `locale` and `copyright`. Write a real `description` in
   `config/_default/params.toml` while you are there.

4. **Set the brand** in `assets/css/tokens/theme.css`. Usually one line:

   ```css
   --accent-hue: 25;      /* 0 to 360 */
   --accent-chroma: 0.15; /* 0 is grey, 0.12 to 0.2 is saturated */
   ```

   Ramps, hover and active states, dark mode and elevation are derived from
   those.

5. **Replace the placeholder icon** at `static/icon.svg`.

6. **Write a page.**

   ```sh
   hugo new content posts/hello.md
   hugo server
   ```

7. **Connect Netlify**, pointing at this repository. `netlify.toml` already has
   the build command, the pinned versions and the image cache directory.

8. **Install the Renovate app** on the repository. `renovate.json` extends the
   shared preset, so Hugo and Go bumps arrive as one pull request covering
   every file that pins them, and hugo-base bumps arrive with their migration
   notes.

## What comes from hugo-base

Layouts, the page shell, design tokens, typography, the image pipeline, the
head and structured data, shortcodes, archetypes, the CI workflow, the shared
conventions and the checks. Read its
[README](https://github.com/bradryanbice/hugo-base#readme) for the public API,
the theme inputs and the content rules.

Files in this repository that hugo-base owns are listed in
`.hugo-base-managed`. Do not edit them: change them upstream, then

```sh
./hugo-base.sh sync
```

CI fails when they are stale, which is how a new convention arrives.

## Checks

```sh
hugo build --gc --minify --panicOnWarning   # the build CI runs
./hugo-base.sh sync --check                 # managed files current
./hugo-base.sh lint                         # CSS policy
./hugo-base.sh quality public               # head, accessibility, contrast, Lighthouse
```

The gate checks the document head on every page, runs axe over every URL in
both colour schemes, verifies every semantic colour pair for contrast, and runs
Lighthouse. It is strict on purpose: it is what makes an automated Hugo upgrade
safe to merge.

## Updating

```sh
hugo mod get -u github.com/bradryanbice/hugo-base
hugo mod tidy
./hugo-base.sh sync
```

Never run `go mod tidy`: there is no Go code here, so it would drop the
hugo-base requirement.

A release that needs work in this site ships a migration note. Apply it with
the `hugo-base-upgrade` skill, which `sync` put in `.claude/skills/`, or follow
the note by hand.

## What this template deliberately does not contain

No layouts, no CSS beyond the theme inputs, no partials except the icons
override, and no CI definition of its own. If you find yourself copying
something out of hugo-base to change it, the seam you need is probably missing,
which is an issue there rather than a local patch.
