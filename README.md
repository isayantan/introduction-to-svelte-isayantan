# Svelte tutorial

To start the project, run the following commands:


```bash
npm create svelte@latest .
```

When prompted, select the following:

- Create the project in the current directory
- Choose continue
- Choose Skeleton project
- Use TypeScript
- Choose the default options for the rest of the questions

After the project is created, we will set up the project to use Tailwind CSS.

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
`

In `svite.config.js`, add the following:

```js
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';
/** @type {import('@sveltejs/kit').Config} */
const config = {
  kit: {
    adapter: adapter()
  },
  preprocess: vitePreprocess()
};
export default config;
```

In `tailwind.config.js`, add the following:

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {}
  },
  plugins: []
};
```

Create a `./src/app.css` file and add the following:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Create a `./src/routes/+layout.svelte` file and add the following:

```html
<script>
  import '../app.css';
</script>

<slot />
```