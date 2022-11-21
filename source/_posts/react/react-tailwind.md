---
title: Create React Project with Tailwind
date: 2022-11-21 21:42:57
categories: Frontend
description: Procedure of creating a tailwind react project
tags:
  - React
  - Tailwind

---

Create a react project with tailwind

* Create project using `create-react-app`.
``` sh
  npx create-react-app my-project
  cd my-project
```

* Install Tailwind CSS and its dependencies. Create `tailwind.config.js` and `postcss.config.js`
``` sh
  npm install -D tailwindcss postcss autoprefixer
  npx tailwindcss init -p
```

* Configure `tailwind.config.js`
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

* Add the Tailwind directives to React css file `(./src/index.css)`
``` css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
* Start React Project
``` sh
npm run start
```

* Modify `App.js`
``` js
export default function App() {
  return (
    <h1 className="text-3xl font-bold underline">
      Hello world!
    </h1>
  )
}
```

* Check whether the `className` work.