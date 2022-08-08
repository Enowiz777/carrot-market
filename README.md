# Carrot Market

# Requirements:

1. NextJS
- NextJS will come with below apps:
    a. React, React-DOM, NextJS

*How do you install NextJS?*
```js
// for typescript
npx create-next-app@latest --typescript
```


2. TaileindCSS

*How do you install?*

Steps:

a. install tailwindcss, postcss, autoprefixer.
```
npm install -D tailwindcss postcss autoprefixer
```

b. initiate tailwindcss
```bash
npx tailwindcss init -p
: << 'COMMENT'
Created Tailwind CSS config file: tailwind.config.js
Created PostCSS config file: postcss.config.js
COMMENT
>>
```

c. Modify tailwind.config.js to match our configurations.

tailwind.config.js
```js
// Inside any folders of page/ and component/ you will use tailwindcss
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};

```

d. Change the styles/globals/css to import tailwindCSS

```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

e. Test it by running below
```bash
npm run dev
```

# Dev journal

20220805
- Added @tailwindcss/forms
```
npm i @tailwindcss/forms
```
- Modify tailwind.config.js to use it

```js
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  darkMode: "media", // class
  plugins: [require("@tailwindcss/forms")],
};

```

# Item Detail page

- If you were to create item pages url, you just need to create a bracket around.

# Build upload screen

- You can just add below path and it will create the path in NextJS.
Path: pages/items/upload.tsx


- File choosing technique


# Create Community page

pages/community/index
- People will go into community and see IDs. 

20220808

# Chats page
divide-y-2 to create a border below

# Chat Detail
- Create a detail page in the chat by doing pages/chat/[id].tsx
- TailwindCSS used:
- w-6
- h-6
- space-x-reverse (reverse the position)
- bottom=0 - put div in the bottom.
- absolute - container father needs to be relative.
- 

# Profile page

- 