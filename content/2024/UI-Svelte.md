+++
title = 'UI - Svelte'
date = 2024-03-26T07:28:11+01:00
draft = false
summary = 'Nightmare countdown initiated...'
+++

The code for this post can be found [here](https://codeberg.org/Tofticles/Anex/pulls/41).

I've been putting this off for quite a while, since it is my least favorite part of developing: Frontend work. It's not that I haven't tried it nor is it that I'm completely terrible at it. It's just.. JavaScript.

But, an API without a frontend makes it hard to show off what you are doing - and Anex is intended to have a UI, so here goes. In the past, I've worked with several different UI frameworks - from WinForms and WPF for the Windows Desktop experience to [Knockback.js](http://kmalakoff.github.io/knockback/)/[Knockout.js](https://github.com/knockout/knockout), [Angular.js](https://angularjs.org/) and [Angular 17](https://angular.io/) for the Web. Angular 17 was the latest and it did get the job done, but is also -heavy-. A lot of moving parts, opinionated choices and The Angular Way - where if you deviate from it, it gets -hard-.

I had [Svelte](https://svelte.dev/) recommended for being lightweight - so, that is what I went with. In contrast to Angular it does not rely on a virtual DOM nor a framework to be downloaded. Everything is compiled into basic HTML/CSS/JS before it is deployed which is a huge plus in my book. For an absolute novice such as me - it is also nice to not have to navigate a massive tree-structure - since each component is intended to live in a single file. This has drawbacks, of course, with reusability, but we'll see what I can wrangle it to do.

I am using [Kit.Svelte](https://kit.svelte.dev/docs/load) on top to make creating things easier - you do need [NodeJS](https://nodejs.org/en) and [NPM](https://www.npmjs.com/), but that holds true for most web frameworks these days, but having those, this is basically what you need to create a Svelte project:

```
npm create svelte@latest my-app
cd my-app
npm install
npm run dev
```

Routes are defined with your directory structure, which makes it nice and simple to keep in sync - I struggled a bit with how to load data and the I found the documentation a bit confusing since you can do it a number of ways. I ended up using [OpenAPI](https://openapi-ts.pages.dev/introduction) and [OpenAPI.Fetch](https://openapi-ts.pages.dev/openapi-fetch/) on top for creating a sort-of-typed service to the Anex API. I would have liked to have it create a full-fledged service and the corresponding TS-types for the Dto types in the OpenAPI spec, but this will suffice for now.

To get this client created, I downloaded the swagger.json file from the Swagger UI, that the backend produces (this file is in `Anex.Web/src/api`) and then from a terminal:

Installation:
```
npm i -D openapi-typescript@next typescript
npm i openapi-fetch
npm i -D openapi-typescript typescript
```

Creating the client (with the terminal in the directory `Anex.Web`):
```
npx openapi-typescript ./src/api/swagger.json ./src/api/schema.d.ts
```

For using the OpenAPI.Fetch generated client, I used the following code to set the base-uri of the API:
```
import createClient from "openapi-fetch";
import type { paths } from "./schema";

export const anexClient = createClient<paths>({ baseUrl: import.meta.env.VITE_ANEX_BASE_API });
```

One little snag, I hit, is that by default Svelte only allows keys prefixed with 'VITE' to be fetched from the env-file. Also, to get intellisense working - I had to add `import type { PageServerLoad } from './$types';` in the +page.server.ts and `import type { PageData } from './$types';` in the +page.svelte code. But with all of that out of the way - I now have the first view rendering data from the API:

![First very basic UI showing fiscal periods](/images/2024/Svelte-first-ui.png)

Of course, there is much work to be done, but this is the first important step.