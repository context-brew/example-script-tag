# Context Brew — script-tag example (any stack)

One `<script>` tag adds the Send-feedback button to **any** site — no framework, no build step.
React is bundled inside the script; styles self-inject.

```html
<script src="https://cdn.cbrew.co/embed.js"
        data-key="fbk_your_publishable_key"
        data-api="https://api.cbrew.co"
        data-name="Acme"
        data-board="https://board.cbrew.co/your-workspace"></script>
```

## Try it

1. Sign up at <https://cbrew.co/#/signup> — copy your **publishable key** (`fbk_…`) and
   **workspace slug**.
2. Edit `index.html`: paste the key into `data-key` and your slug into `data-board`.
3. Serve it: `npx serve .` (or just open `index.html`).
4. Click the 🍴 button → sign in by email + 6-digit code → send feedback → see it on your board.

## This is the path for Vue, Nuxt, Rails, Laravel, Django, …

Put the tag in your layout template so it renders on every page:

- **Rails** — `app/views/layouts/application.html.erb`, before `</body>`.
- **Laravel** — your Blade layout (`resources/views/layouts/app.blade.php`), before `</body>`.
- **Vue** — `index.html` of your SPA.
- **Nuxt 3** — declare it once in config so it rides every page:

  ```ts
  // nuxt.config.ts
  export default defineNuxtConfig({
    app: { head: { script: [{
      src: 'https://cdn.cbrew.co/embed.js',
      'data-key': 'fbk_your_publishable_key',
      'data-name': 'Acme',
      'data-board': 'https://board.cbrew.co/your-workspace',
      tagPosition: 'bodyClose', defer: true,
    }] } },
  })
  ```
- **Anything else** — wherever your pages share a footer.

Prefer to serve the file yourself instead of the CDN? The identical bundle ships inside the
[`cbrew` npm package](https://www.npmjs.com/package/cbrew) at `cbrew/dist/embed.js`
(mirrored at `https://unpkg.com/cbrew/dist/embed.js`). Note it's a **self-contained IIFE, not an
ES module** — serve it as a plain `<script src>` (copy it into `public/`/`app/assets`); don't
importmap-pin it or `import` it into a bundle.

### Tag attributes

| Attribute | Meaning |
|---|---|
| `data-key` | your publishable app key (`fbk_…`) — **required**, browser-safe, origin-locked |
| `data-api` | the hosted API (default `https://api.cbrew.co`) |
| `data-name` | workspace name shown in the popover (optional) |
| `data-board` | "View all feedback" target (optional) |
| `data-context` | prefix stamped on submitted context, e.g. `"storefront "` (optional) |
| `data-howitworks` | `"false"` hides the "How feedback works?" link (default shown) |

Using React? You want the [`cbrew` package](https://www.npmjs.com/package/cbrew) directly —
see the [Vite example](https://github.com/context-brew/example-react-vite) and the
[Next.js example](https://github.com/context-brew/example-nextjs). Full docs: <https://cbrew.co/#/docs>
