# Initial Project Setup with React, Vite, Tailwind CSS, DaisyUI, React Router, and Custom Font

## Step 1: Create a New React Project with Vite

Start by creating a new React project using Vite:

```bash
npm create vite@latest my-project
```

1. Replace `my-project` with your preferred project name.
2. Select **React** as the framework when prompted.

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

---

## Step 3: Install DaisyUI

DaisyUI is a Tailwind CSS plugin that provides pre-styled components. Install it with the following command:

```bash
npm i -D daisyui@latest
```

---

## Step 4: Set Up Tailwind and DaisyUI in `index.css`

To keep everything simple and follow your preference of using just `@import "tailwindcss";`, you can import **Tailwind CSS** and **DaisyUI** directly in the **`index.css`** file.

1. In your **`index.css`**, just add this line to import **Tailwind CSS** and DaisyUI.

```css
/* src/index.css */
@import "tailwindcss"; /* Import Tailwind CSS */
@import "daisyui"; /* Import DaisyUI Plugin */
```

This will configure **Tailwind CSS** and **DaisyUI** without modifying the `tailwind.config.js` file.

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

---

## Step 6: Install React Router

To enable routing in your app, install **React Router**:

```bash
npm i react-router
```

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

---

## Step 9: Run the Development Server

Finally, run the development server to start your app:

```bash
npm run dev
```

Visit `http://localhost:port` in your browser to see the app in action.

---

## Summary

You have successfully set up a React project using **Vite**, **Tailwind CSS**, **DaisyUI**, **React Router**, and a **custom font** (Urbanist). Your app now has routing, styling, and a custom font ready to go, with DaisyUI components available for use.

Enjoy building your app! 🎉

This is the **final Markdown** with all the steps you need to get started with your React project using Vite, Tailwind CSS, DaisyUI, React Router, and the Urbanist font.
