# Nuxt 3 天氣app

Look at the [nuxt 3 documentation](https://v3.nuxtjs.org) to learn more.
## 專案說明 : 
## UI框架-TailwindCSS

## Setup

Make sure to install the dependencies:

```bash
# npm
npm install
```

## Development Server

Start the development server on http://localhost:3000

```bash
npm run dev
```

## Production

Build the application for production:

```bash
npm run build
```

Locally preview production build:

```bash
npm run preview
```
Checkout the [deployment documentation](https://v3.nuxtjs.org/guide/deploy/presets) for more information.
## 如何使用env參數?
### 1.建立env檔案
```bash
//.env
WEATER_API_KEY=4c59c04a41
```
### 2.設置nuxt.config.Ts
```ts
import { defineNuxtConfig } from "nuxt";
// https://v3.nuxtjs.org/api/configuration/nuxt.config
export default defineNuxtConfig({
    publicRuntimeConfig: {
        WEATER_API_KEY: process.env.WEATER_API_KEY
    },
});
```
### 3.在元件中引用
```ts
const config = useRuntimeConfig();
console.log(config.WEATER_API_KEY)
```
