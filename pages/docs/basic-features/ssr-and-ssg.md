---
title: SSR & SSG
authors:
  - ije
  - Serdar Sever
---

# SSR & SSG

By default, Aleph.js **pre-renders** every page. This means that Aleph.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. SSR (server-side rendering) can result in better performance and SEO.

Each generated HTML is associated with minimal JavaScript code necessary for that page. When a page is loaded by the browser, its JavaScript code runs and makes the page fully interactive. (This process is called _hydration_.)

You can disable the **SSR** function in `aleph.config.js`:

```javascript
export default {
  "ssr": false, // SPA mode
  ...
}
```

or specify exclude paths:

```javascript
export default {
  "ssr": {
    "exclude": [
      "/admin/"
      "/dashboard/"
    ]
  },
  ...
}
```

## SSR Data Fetching

To fetch data during **build (SSR) time**, you can use the [`useDeno`](/docs/api-reference/mod.ts#useDeno) hook that can get the **Deno** runtime in your component:

```jsx{5-7}
import React from "https://esm.sh/react"
import { useDeno } from "https://deno.land/x/aleph/mod.ts"

export default function Page() {
  const version = useDeno(() => {
    return Deno.version
  })

  return (
    <p>Powered by Deno v{version.deno}</p>
  )
}
```

or fetching data **asynchronously**:

```jsx
import React from "https://esm.sh/react"
import { useDeno, useRouter } from "https://deno.land/x/aleph/mod.ts"

export default function Post() {
  const { params } = useRouter()
  const post = useDeno(async () => {
    return await (await fetch(`https://.../post/${params.id}`)).json()
  }, true, [params]) // true means to refetch data in the browser deps the `params`

  return (
    <h1>{post.title}</h1>
  )
}
```

> To learn more `useDeno`, check the [useDeno Hook documentation](/docs/advanced-features/use-deno-hook).

## Static Site Generation (SSG)

Aleph.js allows you to build your app to a **static site**, which can be hosted as static html pages on any server or CDN.

```bash
$ aleph build
```

For **dynamic routes**, your can define the `staticPaths` in the `aleph.config.js`:

```javascript
export default async () => {
  const posts = await (await fetch("https://.../posts")).json()
  return {
    ssr: {
      exclude: [...],
      staticPaths: posts.map(({id}) => `/post/${id}`)
    }
  }
}
```

> See the [hello-world](https://alephjs-hello-world.vercel.app/) example on [Vercel](https://vercel.com).
