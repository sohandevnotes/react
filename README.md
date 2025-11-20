# 🚀 Initial Project Setup with React, Vite, Tailwind CSS, DaisyUI, React Router, and Custom Font

## Step 1: Create a New React Project with Vite

Start by creating a new React project using Vite:

```bash
npm create vite@latest my-project
```

1. Replace `my-project` with your preferred project name.
2. Select **React** as the framework when prompted.

🎉 Your new React project is ready!

---

## Step 2: Install Tailwind CSS and Vite Plugin

Navigate into your project folder:

```bash
cd my-project
```

Install **Tailwind CSS** and the **@tailwindcss/vite** plugin:

```bash
npm install tailwindcss @tailwindcss/vite
```

⚡ Tailwind is now installed and ready for use!

---

## Step 3: Install DaisyUI

DaisyUI is a Tailwind CSS plugin that provides pre-styled components. Install it with the following command:

```bash
npm i -D daisyui@latest
```

✨ DaisyUI is now installed for quick styling!

---

## Step 4: Set Up Tailwind and DaisyUI in `index.css`

To keep everything simple and follow your preference of using just `@import "tailwindcss";`, you can import **Tailwind CSS** and **DaisyUI** directly in the **`index.css`** file.

1. In your **`index.css`**, just add this line to import **Tailwind CSS** and DaisyUI.

```css
/* src/index.css */
@import "tailwindcss"; /* Import Tailwind CSS */
@import "daisyui"; /* Import DaisyUI Plugin */
```

🌈 Tailwind and DaisyUI are ready for use in your styles!

---

## Step 5: Add Custom Font - Urbanist

To add a custom font, **Urbanist**, follow these steps:

1. **Import the font** from Google Fonts.
2. **Apply the font** globally to the body of your app.

In your **`index.css`**, add the following:

```css
/* src/index.css */

/* Import Urbanist font from Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Urbanist:ital,wght@0,100..900;1,100..900&display=swap');

/* Apply the font to the body */
body {
    font-family: "Urbanist", sans-serif;
    font-optical-sizing: auto;
    font-style: normal;
}
```

🖋️ **Urbanist** font is now applied to your app for a sleek and modern look!

---

## Step 6: Install React Router

To enable routing in your app, install **React Router**:

```bash
npm i react-router
```

🛣️ Routing is now set up and ready for use!

---

## Step 7: Set Up Routing

Create a `routes` folder inside the `src` directory and inside it, create a file called `routes.jsx`. This will hold the routing configuration for your app.

```javascript
// src/routes/routes.jsx
import { createBrowserRouter } from "react-router";

export const router = createBrowserRouter([
  {
    path: "/",
    element: <div>Hello World</div>,
  },
]);
```

🧭 Your routing structure is set!

---

## Step 8: Update `main.jsx` to Use RouterProvider

Next, open the `main.jsx` file and wrap your app in the `RouterProvider` component to enable routing.

```javascript
// src/main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { RouterProvider } from "react-router";
import { router } from "./routes/routes";
import "./index.css";  // Tailwind CSS, DaisyUI, and Urbanist font are imported here

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

🔌 Routing is now integrated into your app!

---

## Step 9: Run the Development Server

Finally, run the development server to start your app:

```bash
npm run dev
```

🎉 Visit `http://localhost:port` in your browser to see the app in action!

---

## Step 10: (Optional) Install React Icons for Adding Icons

To add icons to your app, you can install **React Icons**:

```bash
npm install react-icons
```

Then, use the icons in your app like this:

```javascript
import { FaReact } from 'react-icons/fa';

function App() {
  return (
    <div>
      <h1>Welcome to your React App!</h1>
      <FaReact size={50} color="#61DAFB" />
    </div>
  );
}
```

🎨 **React Icons** are now ready to use for a more dynamic interface!

---

## Step 11: Run the Development Server Again

Re-run the development server to see all the changes:

```bash
npm run dev
```

🚀 Visit `http://localhost:port` to see your icons and other changes in action!

---

## Summary

🎉 **Congratulations!** You've successfully set up a React project using **Vite**, **Tailwind CSS**, **DaisyUI**, **React Router**, and a **custom font** (Urbanist). Your app now has routing, custom styling, and dynamic icons ready to go.

Enjoy building your app! 🎨💻
