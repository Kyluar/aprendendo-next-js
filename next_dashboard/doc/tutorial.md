# Chapter 1 - Getting Started

## Creating a new project

We recommend using [pnpm] as your package manager, as it's faster and more efficient than `npm` or `yarn`. If you don't have `pnpm` installed, you can install it globally by running:

```sh
npm install -g pnpm
```

To create a Next.js app, open your terminal, [cd] into the folder you'd like to keep your project, and run the following command:

```sh
npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
```

This command uses [create-next-app], a Command Line Interface (CLI) tool that sets up a Next.js application for you. In the command above, you're also using the `--example` flag with the [starter example] for this course.

## Exploring the project

Unlike tutorials that have you write code from scratch, much of the code for this course is already written for you. This better reflects real-world development, where you'll likely be working with existing codebases.

Our goal is to help you focus on learning the main features of Next.js, without having to write all the application code.

After installation, open the project in your code editor and navigate to nextjs-dashboard.

```sh
cd nextjs-dashboard
```

## Folder structure

You'll notice that the project has the following folder structure:
* **`/app`**: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
* **`/app/lib`**: Contains functions used in your application, such as reusable utility functions and data fetching functions.
* **`/app/ui`**: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.
* **`/public`**: Contains all the static assets for your application, such as images.
* **Config Files**: You'll also notice config files such as `next.config.ts` at the root of your application. Most of these files are created and pre-configured when you start a new project using `create-next-app`. You will not need to modify them in this course.

Feel free to explore these folders, and don't worry if you don't understand everything the code is doing yet.

## Placeholder data

When you're building user interfaces, it helps to have some placeholder data. If a database or API is not yet available, you can:
* Use placeholder data in JSON format or as JavaScript objects.
* Use a 3rd party service like [mockAPI].

For this project, we've provided some placeholder data in `app/lib/placeholder-data.ts`. Each JavaScript object in the file represents a table in your database. For example, for the invoices table:

```js
const invoices = [
  {
    customer_id: customers[0].id,
    amount: 15795,
    status: 'pending',
    date: '2022-12-06',
  },
  {
    customer_id: customers[1].id,
    amount: 20348,
    status: 'pending',
    date: '2022-11-14',
  },
  // ...
];
```

In the chapter on setting up your database, you'll use this data to seed your database (populate it with some initial data).

## TypeScript

You may also notice most files have a `.ts` or `.tsx` suffix. This is because the project is written in TypeScript. We wanted to create a course that reflects the modern web landscape.

It's okay if you don't know TypeScript - we'll provide the TypeScript code snippets when required.

For now, take a look at the `/app/lib/definitions.ts` file. Here, we manually define the types that will be returned from the database. For example, the invoices table has the following types:

```ts
export type Invoice = {
  id: string;
  customer_id: string;
  amount: number;
  date: string;
  // In TypeScript, this is called a string union type.
  // It means that the "status" property can only be one of the two strings: 'pending' or 'paid'.
  status: 'pending' | 'paid';
};
```

By using TypeScript, you can ensure you don't accidentally pass the wrong data format to your components or database, like passing a `string` instead of a `number` to invoice `amount`.

## Running the development server

Run `pnpm i` to install the project's packages.

```sh
pnpm i
```

Followed by `pnpm dev` to start the development server.

```sh
pnpm dev
```

`pnpm dev` starts your Next.js development server on port `3000`. Let's check to see if it's working.

Open http://localhost:3000 on your browser. Your home page should look like this, which is intentionally unstyled:

![Home page intentionally unstyled](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Facme-unstyled.png&w=1920&q=75)



# Chapter 2 - CSS Styling

Currently, your home page doesn't have any styles. Let's look at the different ways you can style your Next.js application.

Here are the topics we’ll cover:
* How to add a global CSS file to your application.
* Two different ways of styling: Tailwind and CSS modules.
* How to conditionally add class names with the `clsx` utility package.

## Global styles

If you look inside the `/app/ui` folder, you'll see a file called `global.css`. You can use this file to add CSS rules to **all** the routes in your application - such as CSS reset rules, site-wide styles for HTML elements like links, and more.

You can import `global.css` in any component in your application, but it's usually good practice to add it to your top-level component. In Next.js, this is the [root layout] (more on this later).

Add global styles to your application by navigating to `/app/layout.tsx` and importing the `global.css` file:

```tsx
import '@/app/ui/global.css';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

With the development server still running, save your changes and preview them in the browser. Your home page should now look like this:

![Preview](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fhome-page-with-tailwind.png&w=1080&q=75)

But wait a second, you didn't add any CSS rules, where did the styles come from?

If you take a look inside `global.css`, you'll notice some `@tailwind` directives:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Tailwind

[Tailwind] is a CSS framework that speeds up the development process by allowing you to quickly write [utility classes] directly in your React code.

In Tailwind, you style elements by adding class names. For example, adding `"text-blue-500"` will turn the `<h1>` text blue:

```tsx
<h1 className="text-blue-500">I'm blue!</h1>
```

Although the CSS styles are shared globally, each class is singularly applied to each element. This means if you add or delete an element, you don't have to worry about maintaining separate stylesheets, style collisions, or the size of your CSS bundle growing as your application scales.

When you use `create-next-app` to start a new project, Next.js will ask if you want to use Tailwind. If you select `yes`, Next.js will automatically install the necessary packages and configure Tailwind in your application.

If you look at `/app/page.tsx`, you'll see that we're using Tailwind classes in the example.

```tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
 
export default function Page() {
  return (
    // These are Tailwind classes:
    <main className="flex min-h-screen flex-col p-6">
      <div className="flex h-20 shrink-0 items-end rounded-lg bg-blue-500 p-4 md:h-52">
    // ...
  )
}
```

Don't worry if this is your first time using Tailwind. To save time, we've already styled all the components you'll be using.

Let's play with Tailwind! Copy the code below and paste it above the `<p>` element in `/app/page.tsx`:

```tsx
<div
  className="relative w-0 h-0 border-l-[15px] border-r-[15px] border-b-[26px] border-l-transparent border-r-transparent border-b-black"
/>
```

If you prefer writing traditional CSS rules or keeping your styles separate from your JSX - CSS Modules are a great alternative.

## CSS Modules

[CSS Modules] allow you to scope CSS to a component by automatically creating unique class names, so you don't have to worry about style collisions as well.

We'll continue using Tailwind in this course, but let's take a moment to see how you can achieve the same results from the quiz above using CSS modules.

Inside `/app/ui`, create a new file called `home.module.css` and add the following CSS rules:

```css
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

Then, inside your `/app/page.tsx` file import the styles and replace the Tailwind class names from the `<div>` you've added with `styles.shape`:

```tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import styles from '@/app/ui/home.module.css';
 
export default function Page() {
  return (
    <main className="flex min-h-screen flex-col p-6">
      <div className={styles.shape} />
    // ...
  )
}
```

Save your changes and preview them in the browser. You should see the same shape as before.

Tailwind and CSS modules are the two most common ways of styling Next.js applications. Whether you use one or the other is a matter of preference - you can even use both in the same application!

## Using the `clsx` library to toggle class names

There may be cases where you may need to conditionally style an element based on state or some other condition.

[clsx] is a library that lets you toggle class names easily. We recommend taking a look at [documentation] for more details, but here's the basic usage:
* Suppose that you want to create an `InvoiceStatus` component which accepts `status`. The status can be `'pending'` or `'paid'`.
* If it's `'paid'`, you want the color to be green. If it's `'pending'`, you want the color to be gray.

You can use `clsx` to conditionally apply the classes, like this:

```tsx
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
```

## Other styling solutions

In addition to the approaches we've discussed, you can also style your Next.js application with:
* Sass which allows you to import `.css` and `.scss` files.
* CSS-in-JS libraries such as [styled-jsx], [styled-components], and [emotion].

Take a look at the [CSS documentation] for more information.



# Chapter 3 - Optimizing Fonts and Images

In the previous chapter, you learned how to style your Next.js application. Let's continue working on your home page by adding a custom font and a hero image.

Here are the topics we’ll cover:
* How to add custom fonts with `next/font`.
* How to add images with `next/image`.
* How fonts and images are optimized in Next.js.

## Why optimize fonts?

Fonts play a significant role in the design of a website, but using custom fonts in your project can affect performance if the font files need to be fetched and loaded.

[Cumulative Layout Shift] is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

![](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Ffont-layout-shift.png&w=1920&q=75)

Next.js automatically optimizes fonts in the application when you use the `next/font` module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

## Adding a primary font

Let's add a custom Google font to your application to see how this works.

In your `/app/ui` folder, create a new file called `fonts.ts`. You'll use this file to keep the fonts that will be used throughout your application.

Import the `Inter` font from the `next/font/google` module - this will be your primary font. Then, specify what [subset] you'd like to load. In this case, `'latin'`:

```ts
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
```

Finally, add the font to the <body> element in /app/layout.tsx:

```tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

By adding `Inter` to the `<body>` element, the font will be applied throughout your application. Here, you're also adding the Tailwind [antialiased] class which smooths out the font. It's not necessary to use this class, but it adds a nice touch.

Navigate to your browser, open dev tools and select the `body` element. You should see `Inter` and `Inter_Fallback` are now applied under styles.

## Practice: Adding a secondary font

You can also add fonts to specific elements of your application.

Now it's your turn! In your `fonts.ts` file, import a secondary font called `Lusitana` and pass it to the `<p>` element in your `/app/page.tsx` file. In addition to specifying a subset like you did before, you should also specify different font **weights**. For example, `400` (normal) and `700` (bold).

Once you're ready, expand the code snippet below to see the solution.

**Hints**:
* If you're unsure what weight options to pass to a font, check the TypeScript errors in your code editor.
* Visit the Google Fonts website and search for Lusitana to see what options are available.
* See the documentation for adding multiple fonts and the full list of options.

Finally, the `<AcmeLogo />` component also uses Lusitana. It was commented out to prevent errors, you can now uncomment it:

```tsx
// ...
 
export default function Page() {
  return (
    <main className="flex min-h-screen flex-col p-6">
      <div className="flex h-20 shrink-0 items-end rounded-lg bg-blue-500 p-4 md:h-52">
        <AcmeLogo />
        {/* ... */}
      </div>
    </main>
  );
}
```

Great, you've added two custom fonts to your application! Next, let's add a hero image to the home page.

## Why optimize images?

Next.js can serve **static assets**, like images, under the top-level [/public] folder. Files inside `/public` can be referenced in your application.

With regular HTML, you would add an image as follows:

```html
<img
  src="/hero.png"
  alt="Screenshots of the dashboard project showing desktop version"
/>
```

However, this means you have to manually:
* Ensure your image is responsive on different screen sizes.
* Specify image sizes for different devices.
* Prevent layout shift as the images load.
* Lazy load images that are outside the user's viewport.

Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the `next/image` component to automatically optimize your images.

## The `<Image>` component

The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimization, such as:
* Preventing layout shift automatically when images are loading.
* Resizing images to avoid shipping large images to devices with a smaller viewport.
* Lazy loading images by default (images load as they enter the viewport).
* Serving images in modern formats, like [WebP] and [AVIF], when the browser supports it.

## Adding the desktop hero image

Let's use the `<Image>` component. If you look inside the `/public` folder, you'll see there are two images: `hero-desktop.png` and `hero-mobile.png`. These two images are completely different, and they'll be shown depending if the user's device is a desktop or mobile.

In your `/app/page.tsx` file, import the component from [next/image]. Then, add the image under the comment:

```tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import { lusitana } from '@/app/ui/fonts';
import Image from 'next/image';
 
export default function Page() {
  return (
    // ...
    <div className="flex items-center justify-center p-6 md:w-3/5 md:px-28 md:py-12">
      {/* Add Hero Images Here */}
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
    </div>
    //...
  );
}
```

Here, you're setting the `width` to `1000` and `height` to `760` pixels. It's good practice to set the `width` and `height` of your images to avoid layout shift, these should be an aspect ratio **identical** to the source image. These values are not the size the image is rendered, but instead the size of the actual image file used to understand the aspect ratio.

You'll also notice the class `hidden` to remove the image from the DOM on mobile screens, and `md:block` to show the image on desktop screens.

This is what your home page should look like now:

![Home page](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fhome-page-with-hero.png&w=1080&q=75)

## Practice: Adding the mobile hero image

Now it's your turn! Under the image you've just added, add another `<Image>` component for `hero-mobile.png`.
* The image should have a `width` of `560` and `height` of `620` pixels.
* It should be shown on mobile screens, and hidden on desktop - you can use dev tools to check if the desktop and mobile images are swapped correctly.

Great! Your home page now has a custom font and hero images.

## Recommended reading

There's a lot more to learn about these topics, including optimizing remote images and using local font files. If you'd like to dive deeper into fonts and images, see:
* Image Optimization Docs
* Font Optimization Docs
* Improving Web Performance with Images (MDN)
* Web Fonts (MDN)
* How Core Web Vitals Affect SEO
* How Google handles JavaScript throughout the indexing process



# Chapter 4 - Creating Layouts and Pages

So far, your application only has a home page. Let's learn how you can create more routes with **layouts** and **pages**.

Here are the topics we’ll cover:
* Create the `dashboard` routes using file-system routing.
* Understand the role of folders and files when creating new route segments.
* Create a nested layout that can be shared between multiple dashboard pages.
* Understand what colocation, partial rendering, and the root layout are.

## Nested routing

Next.js uses file-system routing where **folders** are used to create nested routes. Each folder represents a **route segment** that maps to a **URL segment**.

![Diagram showing how folders map to URL segments](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Ffolders-to-url-segments.png&w=1920&q=75)`

You can create separate UIs for each route using `layout.tsx` and `page.tsx` files.

## Page file

`page` is a special Next.js file that exports a React component, and it's required for the route to be accessible.

By having a special name for `page` files, Next.js allows you to [colocate] UI components, test files, and other related code with your routes.

Only the content inside the `page` file will be publicly accessible.

To create a nested route, you can nest folders inside each other and add `page` files inside them. For example:

![Diagram showing how adding a folder called dashboard creates a new route '/dashboard'](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fdashboard-route.png&w=1920&q=75)

## Creating the dashboard page

Create a new folder called `dashboard` inside `/app`. Then, create a new `page.tsx` file inside the `dashboard` folder with the following content:

```tsx
export default function Page() {
  return <p>Dashboard Page</p>;
}
```

Now, make sure that the development server is running and visit http://localhost:3000/dashboard. You should see the "Dashboard Page" text.

This is how you can create different pages in Next.js: create a new route segment using a folder, and add a `page` file inside it.

## Practice: Creating the dashboard pages

Let's practice creating more routes. In your dashboard, create two more pages:
* **Customers Page**: The page should be accessible on http://localhost:3000/dashboard/customers. For now, it should return a `<p>Customers Page</p>` element.
* **Invoices Page**: The invoices page should be accessible on http://localhost:3000/dashboard/invoices. For now, also return a `<p>Invoices Page</p>` element.

## Layout file

In Next.js, you can use a special `layout.tsx` file to create UI that is shared between multiple pages.

On navigation, only the page components update while the layout won't re-render. This is called [partial rendering] which preserves client-side React state in the layout when transitioning between pages.

## Creating the dashboard layout

Let's create a layout for the dashboard pages. Inside the `/dashboard` folder, add a new file called `layout.tsx` and paste the following code:

```tsx
import SideNav from '@/app/ui/dashboard/sidenav';
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
```

A few things are going on in this code, so let's break it down:

First, you're importing the `<SideNav />` component into your layout. Any components you import into this file will be part of the layout.

The `<Layout />` component receives a `children` prop. This child can either be a page or another layout. In your case, the pages inside `/dashboard` will automatically be nested inside a `<Layout />` like so:

![Folder structure with dashboard layout nesting the dashboard pages as children](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fshared-layout.png&w=1920&q=75)

## Root layout

The [root layout] is required in every Next.js application. 

Any UI you add to the root layout will be shared across **all** pages in your application. 

You can use the root layout to modify your `<html>` and `<body>` tags, and add metadata.



# Chapter 5 - Navigating Between Pages

Here are the topics we’ll cover:
* How to use the `next/link` component.
* How to show an active link with the `usePathname()` hook.
* How navigation works in Next.js.

## Why optimize navigation?

To link between pages, you would traditionally use the HTML `<a>` element. Which causes a full page refresh on each page navigation.

## The `<Link>` component

In Next.js, you can use the `<Link />` Component to link between pages in your application. `<Link>` allows you to do [client-side navigation] with JavaScript.

The `Link` component is similar to using `<a>` tags, but instead of `<a href="…">`, you use `<Link href="…">`.

To use the `<Link /> `component, open `/app/ui/dashboard/nav-links.tsx`, and import the Link component from [next/link]. Then, replace the `<a>` tag with `<Link>`:

```tsx
import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
 
// ...
 
export default function NavLinks() {
  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
          <Link
            key={link.name}
            href={link.href}
            className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3"
          >
            <LinkIcon className="w-6" />
            <p className="hidden md:block">{link.name}</p>
          </Link>
        );
      })}
    </>
  );
}
```

## Automatic code-splitting and prefetching

To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React [SPA], where the browser loads all your application code on the initial page load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work. This is also less code for the browser to parse, which makes your application faster.

Furthermore, in production, whenever `<Link>` components appear in the browser's viewport, Next.js automatically **prefetches** the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!



# Chapter 6 - Setting Up Your Database

Before you can continue working on your dashboard, you'll need some data. In this chapter, you'll be setting up a PostgreSQL database from one of [Vercel's marketplace integrations].

Here are the topics we’ll cover:
* Push your project to GitHub.
* Set up a Vercel account and link your GitHub repo for instant previews and deployments.
* Create and link your project to a Postgres database.
* Seed the database with initial data.

## Create a GitHub repository

To start, let's push your repository to GitHub if you haven't already. This will make it easier to set up your database and deploy.

If you need help setting up your repository, take a look at [this guide on GitHub].

## Create a Vercel account

Visit [vercel.com/signup] to create an account. Choose the free "hobby" plan. Select **Continue with GitHub** to connect your GitHub and Vercel accounts.

## Connect and deploy your project

Next, you'll be taken to this screen where you can select and import the GitHub repository you've just created:

![Screenshot of Vercel Dashboard, showing the import project screen with a list of the user's GitHub Repositories](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fimport-git-repo.png&w=1080&q=75)

Name your project and click Deploy.

![Deployment screen showing the project name field and a deploy button](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fconfigure-project.png&w=1080&q=75)

Hooray! 🎉 Your project is now deployed.

![Project overview screen showing the project name, domain, and deployment status](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fdeployed-project.png&w=1080&q=75)

By connecting your GitHub repository, whenever you push changes to your **main** branch, Vercel will automatically redeploy your application with no configuration needed.

When opening pull requests, you'll also have [instant preview URLs] which allow you to catch deployment errors early and share a preview of your project with team members for feedback.

## Create a Postgres database

Next, to set up a database, click **Continue to Dashboard** and select the **Storage** tab from your project dashboard. 

Select **Create Database**. Depending on when your Vercel account was created, you may see options like Neon or Supabase. Choose your preferred provider and click **Continue**.

![Connect Store screen showing the Postgres option along with KV, Blob and Edge Config](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fcreate-database.png&w=1080&q=75)

Choose your region and storage plan, if required. The [default region] for all Vercel projects is **Washington D.C (iad1)**, and we recommend choosing this if available to reduce [latency] for data requests.

![Database creation modal showing the database name and region](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fdatabase-region.png&w=1080&q=75)

Once connected, navigate to the `.env.local` tab, click **Show secret** and **Copy Snippet**. Make sure you reveal the secrets before copying them.

![The .env.local tab showing the hidden database secrets](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fdatabase-dashboard.png&w=1080&q=75)

Navigate to your code editor and rename the `.env.example` file to `.env`. Paste in the copied contents from Vercel.

> **Important**: Go to your `.gitignore` file and make sure `.env` is in the ignored files to prevent your database secrets from being exposed when you push to GitHub.

## Seed your database

Now that your database has been created, let's seed it with some initial data.

We've included an API you can access in the browser, which will run a seed script to populate the database with an initial set of data.

The script uses **SQL** to create the tables, and the data from `placeholder-data.ts` file to populate them after they've been created.

Ensure your local development server is running with `pnpm run dev` and navigate to `localhost:3000/seed` in your browser. When finished, you will see a message "Database seeded successfully" in the browser. Once completed, you can delete this file.

### Troubleshooting:

* Make sure to reveal your database secrets before copying it into your `.env` file.
* The script uses `bcrypt` to hash the user's password, if `bcrypt` isn't compatible with your environment, you can update the script to use [bcryptjs] instead.
* If you run into any issues while seeding your database and want to run the script again, you can drop any existing tables by running `DROP TABLE tablename` in your database query interface. See the [executing queries section] below for more details. But be careful, this command will delete the tables and all their data. It's ok to do this with your example app since you're working with placeholder data, but you shouldn't run this command in a production app.

## Executing queries

Let's execute a query to make sure everything is working as expected. We'll use another Router Handler, `app/query/route.ts`, to query the database. Inside this file, you'll find a `listInvoices()` function that has the following SQL query.

```sql
SELECT invoices.amount, customers.name
FROM invoices
JOIN customers ON invoices.customer_id = customers.id
WHERE invoices.amount = 8546;
```

Uncomment the file, remove the `Response.json()` block, and navigate to `localhost:3000/query` in your browser. You should see that an invoice `amount` and `name` is returned.



# Chapter 7 - Fetching Data

Now that you've created and seeded your database, let's discuss the different ways you can fetch data for your application, and build out your dashboard overview page.

Here are the topics we’ll cover
* Learn about some approaches to fetching data: APIs, ORMs, SQL, etc.
* How Server Components can help you access back-end resources more securely.
* What network waterfalls are.
* How to implement parallel data fetching using a JavaScript Pattern.

## Choosing how to fetch data

### API layer

APIs are an intermediary layer between your application code and database. There are a few cases where you might use an API:
* If you're using third-party services that provide an API.
* If you're fetching data from the client, you want to have an API layer that runs on the server to avoid exposing your database secrets to the client.

In Next.js, you can create API endpoints using [Route Handlers].

### Database queries

When you're creating a full-stack application, you'll also need to write logic to interact with your database. For [relational databases] like Postgres, you can do this with SQL or with an [ORM].

There are a few cases where you have to write database queries:
* When creating your API endpoints, you need to write logic to interact with your database.
* If you are using React Server Components (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.

## Using Server Components to fetch data

Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:
* Server Components support JavaScript Promises, providing a solution for asynchronous tasks like data fetching natively. You can use `async/await` syntax without needing `useEffect`, `useState` or other data fetching libraries.
* Server Components run on the server, so you can keep expensive data fetches and logic on the server, only sending the result to the client.
* Since Server Components run on the server, you can query the database directly without an additional API layer. This saves you from writing and maintaining additional code.

## Using SQL

For your dashboard application, you'll write database queries using the [postgres.js] library and SQL. There are a few reasons why we'll be using SQL:
* SQL is the industry standard for querying relational databases (e.g. ORMs generate SQL under the hood).
* Having a basic understanding of SQL can help you understand the fundamentals of relational databases, allowing you to apply your knowledge to other tools.
* SQL is versatile, allowing you to fetch and manipulate specific data.
* The `postgres.js` library provides protection against [SQL injections].

Go to `/app/lib/data.ts`. Here you'll see that we're using `postgres`. The `sql` [function] allows you to query your database:

```ts
import postgres from 'postgres';
 
const sql = postgres(process.env.POSTGRES_URL!, { ssl: 'require' });
 
// ...
```

You can call `sql` anywhere on the server, like a Server Component. But to allow you to navigate the components more easily, we've kept all the data queries in the `data.ts` file, and you can import them into the components.

## Fetching data for the dashboard overview page

Now that you understand different ways of fetching data, let's fetch data for the dashboard overview page. Navigate to `/app/dashboard/page.tsx`, paste the following code, and spend some time exploring it:

```tsx
import { Card } from '@/app/ui/dashboard/cards';
import RevenueChart from '@/app/ui/dashboard/revenue-chart';
import LatestInvoices from '@/app/ui/dashboard/latest-invoices';
import { lusitana } from '@/app/ui/fonts';
 
export default async function Page() {
  return (
    <main>
      <h1 className={`${lusitana.className} mb-4 text-xl md:text-2xl`}>
        Dashboard
      </h1>
      <div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-4">
        {/* <Card title="Collected" value={totalPaidInvoices} type="collected" /> */}
        {/* <Card title="Pending" value={totalPendingInvoices} type="pending" /> */}
        {/* <Card title="Total Invoices" value={numberOfInvoices} type="invoices" /> */}
        {/* <Card
          title="Total Customers"
          value={numberOfCustomers}
          type="customers"
        /> */}
      </div>
      <div className="mt-6 grid grid-cols-1 gap-6 md:grid-cols-4 lg:grid-cols-8">
        {/* <RevenueChart revenue={revenue}  /> */}
        {/* <LatestInvoices latestInvoices={latestInvoices} /> */}
      </div>
    </main>
  );
}
```
The code above is intentionally commented out. We will now begin to example each piece.
* The `page` is an **async** server component. This allows you to use `await` to fetch data.
* There are also 3 components which receive data: `<Card>`, `<RevenueChart>`, and `<LatestInvoices>`. They are currently commented out and not yet implemented.

### Fetching data for `<RevenueChart/>`

To fetch data for the `<RevenueChart/>` component, import the `fetchRevenue` function from `data.ts` and call it inside your component:

```tsx
import { Card } from '@/app/ui/dashboard/cards';
import RevenueChart from '@/app/ui/dashboard/revenue-chart';
import LatestInvoices from '@/app/ui/dashboard/latest-invoices';
import { lusitana } from '@/app/ui/fonts';
import { fetchRevenue } from '@/app/lib/data';
 
export default async function Page() {
  const revenue = await fetchRevenue();
  // ...
}
```

Next, let's do the following:
1. Uncomment the `<RevenueChart/>` component.
2. Navigate to the component file (`/app/ui/dashboard/revenue-chart.tsx`) and uncomment the code inside it.
3. Check `localhost:3000` and you should see a chart that uses `revenue` data.

![Revenue chart showing the total revenue for the last 12 months](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Frecent-revenue.png&w=1080&q=75)

### Fetching data for `<LatestInvoices/>`

For the `<LatestInvoices />` component, we need to get the latest 5 invoices, sorted by date.

You could fetch all the invoices and sort through them using JavaScript. This isn't a problem as our data is small, but as your application grows, it can significantly increase the amount of data transferred on each request and the JavaScript required to sort through it.

Instead of sorting through the latest invoices in-memory, you can use an SQL query to fetch only the last 5 invoices. For example, this is the SQL query from your `data.ts` file:

```ts
// Fetch the last 5 invoices, sorted by date
const data = await sql<LatestInvoiceRaw[]>`
  SELECT invoices.amount, customers.name, customers.image_url, customers.email
  FROM invoices
  JOIN customers ON invoices.customer_id = customers.id
  ORDER BY invoices.date DESC
  LIMIT 5`;
```

In your page, import the `fetchLatestInvoices` function:

```tsx
import { Card } from '@/app/ui/dashboard/cards';
import RevenueChart from '@/app/ui/dashboard/revenue-chart';
import LatestInvoices from '@/app/ui/dashboard/latest-invoices';
import { lusitana } from '@/app/ui/fonts';
import { fetchRevenue, fetchLatestInvoices } from '@/app/lib/data';
 
export default async function Page() {
  const revenue = await fetchRevenue();
  const latestInvoices = await fetchLatestInvoices();
  // ...
}
```

Then, uncomment the `<LatestInvoices />` component. You will also need to uncomment the relevant code in the `<LatestInvoices />` component itself, located at `/app/ui/dashboard/latest-invoices`.

If you visit your localhost, you should see that only the last 5 are returned from the database. Hopefully, you're beginning to see the advantages of querying your database directly!

![Latest invoices component alongside the revenue chart](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Flatest-invoices.png&w=1080&q=75)

### Practice: Fetch data for the `<Card>` components

Now it's your turn to fetch data for the `<Card>` components. The cards will display the following data:
* Total amount of invoices collected.
* Total amount of invoices pending.
* Total number of invoices.
* Total number of customers.

Again, you might be tempted to fetch all the invoices and customers, and use JavaScript to manipulate the data. For example, you could use Array.length to get the total number of invoices and customers:

```ts
const totalInvoices = allInvoices.length;
const totalCustomers = allCustomers.length;
```

But with SQL, you can fetch only the data you need. It's a little longer than using Array.length, but it means less data needs to be transferred during the request. This is the SQL alternative:

```ts
const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
```

The function you will need to import is called `fetchCardData`. You will need to destructure the values returned from the function.

**Hint**:
* Check the card components to see what data they need.
* Check the `data.ts` file to see what the function returns.

## What are request waterfalls?

A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

![Diagram showing time with sequential data fetching and parallel data fetching](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fsequential-parallel-data-fetching.png&w=1920&q=75)

For example, we need to wait for `fetchRevenue()` to execute before `fetchLatestInvoices()` can start running, and so on.

```tsx
const revenue = await fetchRevenue();
const latestInvoices = await fetchLatestInvoices(); // wait for fetchRevenue() to finish
const {
  numberOfInvoices,
  numberOfCustomers,
  totalPaidInvoices,
  totalPendingInvoices,
} = await fetchCardData(); // wait for fetchLatestInvoices() to finish
```

This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.

However, this behavior can also be unintentional and impact performance.

## Parallel data fetching

A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.

In JavaScript, you can use the [Promise.all()] or [Promise.allSettled()] functions to initiate all promises at the same time. 

For example, in `data.ts`, we're using `Promise.all()` in the `fetchCardData()` function:

```ts
export async function fetchCardData() {
  try {
    // ...
    const data = await Promise.all([
      invoiceCountPromise,
      customerCountPromise,
      invoiceStatusPromise,
    ]);
    // ...
  }
}
```

By using this pattern, you can:
* Start executing all data fetches at the same time, which is faster than waiting for each request to complete in a waterfall.
* Use a native JavaScript pattern that can be applied to any library or framework.

However, there is one **disadvantage** of relying only on this JavaScript pattern: what happens if one data request is slower than all the others? Let's find out more in the next chapter.



# Chapter 8 - Static and Dynamic Rendering

Here are the topics we’ll cover:
* What static rendering is and how it can improve your application's performance.
* What dynamic rendering is and when to use it.
* Different approaches to make your dashboard dynamic.
* Simulate a slow data fetch to see what happens.

## What is Static Rendering?

With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or when [revalidating data].

Whenever a user visits your application, the cached result is served. There are a couple of benefits of static rendering:
* **Faster Websites** - Prerendered content can be cached and globally distributed when deployed to platforms like Vercel. This ensures that users around the world can access your website's content more quickly and reliably.
* **Reduced Server Load** - Because the content is cached, your server does not have to dynamically generate content for each user request. This can reduce compute costs.
* **SEO** - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.

Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data which is regularly updated.

## What is Dynamic Rendering?

With dynamic rendering, content is rendered on the server for each user at **request time** (when the user visits the page). There are a couple of benefits of dynamic rendering:

**Real-Time Data** - Dynamic rendering allows your application to display real-time or frequently updated data. This is ideal for applications where data changes often.
**User-Specific Content** - It's easier to serve personalized content, such as dashboards or user profiles, and update the data based on user interaction.
**Request Time Information** - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.

> With dynamic rendering, **your application is only as fast as your slowest data fetch**.



# Chapter 9 - Streaming

Here are the topics we’ll cover:
* What streaming is and when you might use it.
* How to implement streaming with `loading.tsx` and Suspense.
* What loading skeletons are.
* What Next.js Route Groups are, and when you might use them.
* Where to place React Suspense boundaries in your application.

## What is streaming?

Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

![Diagram showing time with sequential data fetching and parallel data fetching](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fserver-rendering-with-streaming.png&w=1920&q=75)

By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

![Diagram showing time with sequential data fetching and parallel data fetching](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fserver-rendering-with-streaming-chart.png&w=1920&q=75)

Streaming works well with React's component model, as each component can be considered a chunk.

There are two ways you implement streaming in Next.js:
1. At the page level, with the `loading.tsx` file (which creates `<Suspense>` for you).
2. At the component level, with `<Suspense>` for more granular control.

## Streaming a whole page with `loading.tsx`

`loading.tsx` is a special Next.js file built on top of React Suspense. It allows you to create fallback UI to show as a replacement while page content loads.

In the `/app/dashboard` folder, create a new file called `loading.tsx`:

```tsx
export default function Loading() {
  return <div>Loading...</div>;
}
```

Refresh http://localhost:3000/dashboard, and you should now see:

![Dashboard page with 'Loading...' text](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Floading-page.png&w=1080&q=75)


A few things are happening here:
1. Since `<SideNav>` is static, it's shown immediately. The user can interact with `<SideNav>` while the dynamic content is loading.
2. The user doesn't have to wait for the page to finish loading before navigating away (this is called **interruptable navigation**).

## Adding loading skeletons

A loading skeleton is a simplified version of the UI. Many websites use them as a placeholder (or fallback) to indicate to users that the content is loading. 

Any UI you add in loading.tsx will be embedded as part of the static file, and sent first. Then, the rest of the dynamic content will be streamed from the server to the client.

Inside your `loading.tsx` file, import a new component called `<DashboardSkeleton>`:

```tsx
import DashboardSkeleton from '@/app/ui/skeletons';
 
export default function Loading() {
  return <DashboardSkeleton />;
}
```

Then, refresh http://localhost:3000/dashboard, and you should now see:

![Dashboard page with loading skeletons](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Floading-page-with-skeleton.png&w=1080&q=75)

### Fixing the loading skeleton bug with route groups

Since `loading.tsx` is a level higher than `/invoices/page.tsx` and `/customers/page.ts`x in the file system, it's also applied to those pages.

We can change this with [Route Groups]. Create a new folder called `/(overview)` inside the dashboard folder. Then, move your `loading.tsx` and `page.tsx` files inside the folder:

![Folder structure showing how to create a route group using parentheses](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Froute-group.png&w=1920&q=75)

Now, the `loading.tsx` file will only apply to your dashboard overview page.

## Route Groups

[Route groups] allow you to organize files into logical groups without affecting the URL path structure. 

When you create a new folder using parentheses `()`, the name won't be included in the URL path. So `/dashboard/(overview)/page.tsx` becomes `/dashboard`.

Before, you're using a route group to ensure `loading.tsx` only applies to your dashboard overview page. However, you can also use route groups to separate your application into sections (e.g. `(marketing)` routes and `(shop)` routes) or by teams for larger applications.

## Streaming a component

So far, you're streaming a whole page. But you can also be more granular and stream specific components using React Suspense.

> Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). 

You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

To do so, you'll need to import `<Suspense>` from React, and wrap it around the `<RevenueChart>`. You can pass it a fallback component called `<RevenueChartSkeleton>`.

```tsx
// ...
import { Suspense } from 'react';
import { RevenueChartSkeleton } from '@/app/ui/skeletons';
// ...
<Suspense fallback={<RevenueChartSkeleton />}>
  <RevenueChart />
</Suspense>
// ...
```

Now refresh the page, you should see the dashboard information almost immediately, while a fallback skeleton is shown for `<RevenueChart>`:

![Dashboard page with revenue chart skeleton and loaded Card and Latest Invoices components](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Floading-revenue-chart.png&w=1080&q=75)

## Practice: Streaming `<LatestInvoices>`

Now it's your turn! Practice what you've just learned by streaming the `<LatestInvoices>` component.

Move `fetchLatestInvoices()` down from the page to the `<LatestInvoices>` component. Wrap the component in a `<Suspense>` boundary with a fallback called `<LatestInvoicesSkeleton>`.

## Grouping components

Great! You're almost there, now you need to wrap the `<Card>` components in Suspense. You can fetch data for each individual card, but this could lead to a popping effect as the cards load in, this can be visually jarring for the user.

So, how would you tackle this problem?

To create more of a staggered effect, you can group the cards using a wrapper component. This means the static `<SideNav/>` will be shown first, followed by the cards, etc.

In your `page.tsx` file:
1. Delete your `<Card>` components.
2. Delete the `fetchCardData()` function.
3. Import a new **wrapper** component called `<CardWrapper />`.
4. Import a new **skeleton** component called `<CardsSkeleton />`.
5. Wrap `<CardWrapper />` in Suspense.

```tsx
import CardWrapper from '@/app/ui/dashboard/cards';
// ...
import {
  // ...
  CardsSkeleton,
} from '@/app/ui/skeletons';
//...
<Suspense fallback={<CardsSkeleton />}>
  <CardWrapper />
</Suspense>
//...
```

Then, move into the file `/app/ui/dashboard/cards.tsx`, import the `fetchCardData()` function, and invoke it inside the `<CardWrapper/>` component. Make sure to uncomment any necessary code in this component.

```tsx
// ...
import { fetchCardData } from '@/app/lib/data';
// ...
export default async function CardWrapper() {
  const {
    numberOfInvoices,
    numberOfCustomers,
    totalPaidInvoices,
    totalPendingInvoices,
  } = await fetchCardData();
// ...
```

Refresh the page, and you should see all the cards load in at the same time. You can use this pattern when you want multiple components to load in at the same time.



## Deciding where to place your Suspense boundaries

Where you place your Suspense boundaries will depend on a few things:
1. How you want the user to experience the page as it streams.
2. What content you want to prioritize.
3. If the components rely on data fetching.

Take a look at your dashboard page, is there anything you would've done differently?

Don't worry. There isn't a right answer.
* You could stream the **whole page** like we did with `loading.tsx`... but that may lead to a longer loading time if one of the components has a slow data fetch.
* You could stream **every component** individually... but that may lead to UI popping into the screen as it becomes ready.
* You could also create a staggered effect by streaming **page sections**. But you'll need to create wrapper components.

Where you place your suspense boundaries will vary depending on your application. 

In general, it's good practice to move your data fetches down to the components that need it, and then wrap those components in Suspense.

But there is nothing wrong with streaming the sections or the whole page if that's what your application needs.

Don't be afraid to experiment with Suspense and see what works best, it's a powerful API that can help you create more delightful user experiences.



# Chapter 10 - Partial Prerendering

Here are the topics we’ll cover:
* What Partial Prerendering is.
* How Partial Prerendering works.

## Static vs. Dynamic Routes

For most web apps built today, you either choose between static and dynamic rendering for your **entire application**, or for a **specific route**.

In Next.js, if you call a [dynamic function] in a route (like querying your database), the entire route becomes dynamic.

However, most routes are not fully static or dynamic. For example, consider an ecommerce site. You might want to statically render the majority of the product information page, but you may want to fetch the user's cart and recommended products dynamically, this allows you to show personalized content to your users.

## What is Partial Prerendering?

A new rendering model that allows you to combine the benefits of static and dynamic rendering in the same route. For example:

![Partially Prerendered Product Page showing static nav and product information, and dynamic cart and recommended products](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fthinking-in-ppr.png&w=1920&q=75)

When a user visits a route:
* A static route shell that includes the navbar and product information is served, ensuring a fast initial load.
* The shell leaves holes where dynamic content like the cart and recommended products will load in asynchronously.
* The async holes are streamed in parallel, reducing the overall load time of the page.

## How does Partial Prerendering work?

Partial Prerendering uses React's [Suspense] to defer rendering parts of your application until some condition is met (e.g. data is loaded).

The Suspense fallback is embedded into the initial HTML file along with the static content. At build time (or during revalidation), the static content is **prerendered** to create a static shell. The rendering of dynamic content is **postponed** until the user requests the route.

Wrapping a component in Suspense doesn't make the component itself dynamic, but rather Suspense is used as a boundary between your static and dynamic code.

The great thing about Partial Prerendering is that you don't need to change your code to use it. As long as you're using Suspense to wrap the dynamic parts of your route, Next.js will know which parts of your route are static and which are dynamic.

## Implementing Partial Prerendering

Partial Prerendering is an experimental feature introduced in Next.js 14. **Is only available with the Next.js canary releases** (`next@canary`), not in the stable version of Next.js.

To install the canary release of Next.js, run:
```sh
pnpm install next@canary
```
Enable PPR for your Next.js app by adding the [ppr] option to your `next.config.mjs` file:

```ts
import type { NextConfig } from 'next';
 
const nextConfig: NextConfig = {
  experimental: {
    ppr: 'incremental'
  }
};
 
export default nextConfig;
```

The `'incremental'` value allows you to adopt PPR for specific routes.

Next, add the `experimental_ppr` segment config option to your dashboard layout:

```tsx
import SideNav from '@/app/ui/dashboard/sidenav';
 
export const experimental_ppr = true;
 
// ...
```

That's it. You may not see a difference in your application in development, but you should notice a performance improvement in production. 



# Chapter 11 - Adding Search and Pagination

Here are the topics we’ll cover:
* Learn how to use the Next.js APIs: `useSearchParams`, `usePathname`, and `useRouter`.
* Implement search and pagination using URL search params.

## Starting code

Inside your `/dashboard/invoices/page.tsx` file, paste the following code:

```tsx
import Pagination from '@/app/ui/invoices/pagination';
import Search from '@/app/ui/search';
import Table from '@/app/ui/invoices/table';
import { CreateInvoice } from '@/app/ui/invoices/buttons';
import { lusitana } from '@/app/ui/fonts';
import { InvoicesTableSkeleton } from '@/app/ui/skeletons';
import { Suspense } from 'react';
 
export default async function Page() {
  return (
    <div className="w-full">
      <div className="flex w-full items-center justify-between">
        <h1 className={`${lusitana.className} text-2xl`}>Invoices</h1>
      </div>
      <div className="mt-4 flex items-center justify-between gap-2 md:mt-8">
        <Search placeholder="Search invoices..." />
        <CreateInvoice />
      </div>
      {/*  <Suspense key={query + currentPage} fallback={<InvoicesTableSkeleton />}>
        <Table query={query} currentPage={currentPage} />
      </Suspense> */}
      <div className="mt-5 flex w-full justify-center">
        {/* <Pagination totalPages={totalPages} /> */}
      </div>
    </div>
  );
}
```

Spend some time familiarizing yourself with the page and the components you'll be working with:
1. `<Search/>` allows users to search for specific invoices.
2. `<Pagination/>` allows users to navigate between pages of invoices.
3. `<Table/>` displays the invoices.

Your search functionality will span the client and the server. When a user searches for an invoice on the client, the URL params will be updated, data will be fetched on the server, and the table will re-render on the server with the new data.


## Why use URL search params?

As mentioned above, you'll be using URL search params to manage the search state.

There are a couple of benefits of implementing search with URL params:
* **Bookmarkable and shareable URLs**: Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.
* **Server-side rendering**: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.
* **Analytics and tracking**: Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.

## Adding the search functionality

These are the Next.js client hooks that you'll use to implement the search functionality:
* `useSearchParams`- Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
* `usePathname` - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, `usePathname` would return `'/dashboard/invoices'`.
* `useRouter` - Enables navigation between routes within client components programmatically. There are [multiple methods] you can use.

Here's a quick overview of the implementation steps:
1. Capture the user's input.
2. Update the URL with the search params.
3. Keep the URL in sync with the input field.
4. Update the table to reflect the search query.

### 1. Capture the user's input

Go into the `<Search>` Component (`/app/ui/search.tsx`), and you'll notice:
* `"use client"` - This is a Client Component, which means you can use event listeners and hooks.
* `<input>` - This is the search input.

Create a new `handleSearch` function, and add an `onChange` listener to the `<input>` element. `onChange` will invoke `handleSearch` whenever the input value changes.

```tsx
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
 
export default function Search({ placeholder }: { placeholder: string }) {
  function handleSearch(term: string) {
    console.log(term);
  }
 
  return (
    <div className="relative flex flex-1 flex-shrink-0">
      <label htmlFor="search" className="sr-only">
        Search
      </label>
      <input
        className="peer block w-full rounded-md border border-gray-200 py-[9px] pl-10 text-sm outline-2 placeholder:text-gray-500"
        placeholder={placeholder}
        onChange={(e) => {
          handleSearch(e.target.value);
        }}
      />
      <MagnifyingGlassIcon className="absolute left-3 top-1/2 h-[18px] w-[18px] -translate-y-1/2 text-gray-500 peer-focus:text-gray-900" />
    </div>
  );
}
```

Verify that it's working correctly by opening the console in your browser developer tools, then type into the search field. You should see the search term logged to the browser console.

Great! You're capturing the user's search input. Now, you need to update the URL with the search term.

### 2. Update the URL with the search params

Import the `useSearchParams` hook from `next/navigation` and assign it to a variable:

```tsx
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { useSearchParams } from 'next/navigation';
 
export default function Search() {
  const searchParams = useSearchParams();
 
  function handleSearch(term: string) {
    console.log(term);
  }
  // ...
}
```

Inside `handleSearch`, create a new [URLSearchParams] instance using your new `searchParams` variable.

```tsx
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { useSearchParams } from 'next/navigation';
 
export default function Search() {
  const searchParams = useSearchParams();
 
  function handleSearch(term: string) {
    const params = new URLSearchParams(searchParams);
  }
  // ...
}
```

`URLSearchParams` is a Web API that provides utility methods for manipulating the URL query parameters. Instead of creating a complex string literal, you can use it to get the params string like `?page=1&query=a`.

Next, `set` the params string based on the user’s input. If the input is empty, you want to `delete` it:

```tsx
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { useSearchParams } from 'next/navigation';
 
export default function Search() {
  const searchParams = useSearchParams();
 
  function handleSearch(term: string) {
    const params = new URLSearchParams(searchParams);
    if (term) {
      params.set('query', term);
    } else {
      params.delete('query');
    }
  }
  // ...
}
```

Now that you have the query string. You can use Next.js's `useRouter` and `usePathname` hooks to update the URL.

Import `useRouter` and `usePathname` from `'next/navigation'`, and use the replace method from `useRouter()` inside `handleSearch`:

```tsx
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { useSearchParams, usePathname, useRouter } from 'next/navigation';
 
export default function Search() {
  const searchParams = useSearchParams();
  const pathname = usePathname();
  const { replace } = useRouter();
 
  function handleSearch(term: string) {
    const params = new URLSearchParams(searchParams);
    if (term) {
      params.set('query', term);
    } else {
      params.delete('query');
    }
    replace(`${pathname}?${params.toString()}`);
  }
}
```

Here's a breakdown of what's happening:
* `${pathname}` is the current path, in your case, `"/dashboard/invoices"`.
* As the user types into the search bar, `params.toString()` translates this input into a URL-friendly format.
* `replace(${pathname}?${params.toString()})` updates the URL with the user's search data. For example, `/dashboard/invoices?query=lee` if the user searches for "Lee".
* The URL is updated without reloading the page, thanks to Next.js's client-side navigation (which you learned about in the chapter on [navigating between pages]).

### 3. Keeping the URL and input in sync

To ensure the input field is in sync with the URL and will be populated when sharing, you can pass a `defaultValue` to input by reading from `searchParams`:

```tsx
<input
  className="peer block w-full rounded-md border border-gray-200 py-[9px] pl-10 text-sm outline-2 placeholder:text-gray-500"
  placeholder={placeholder}
  onChange={(e) => {
    handleSearch(e.target.value);
  }}
  defaultValue={searchParams.get('query')?.toString()}
/>
```

> **`defaultValue` vs. `value` / Controlled vs. Uncontrolled**
> 
> If you're using state to manage the value of an input, you'd use the `value` attribute to make it a controlled component. This means React would manage the input's state.
>
> However, since you're not using state, you can use `defaultValue`. This means the native input will manage its own state. This is okay since you're saving the search query to the URL instead of state.

### 4. Updating the table

Finally, you need to update the table component to reflect the search query.

Navigate back to the invoices page.

Page components [accept a prop called searchParams], so you can pass the current URL params to the `<Table>` component.

```tsx
import Pagination from '@/app/ui/invoices/pagination';
import Search from '@/app/ui/search';
import Table from '@/app/ui/invoices/table';
import { CreateInvoice } from '@/app/ui/invoices/buttons';
import { lusitana } from '@/app/ui/fonts';
import { Suspense } from 'react';
import { InvoicesTableSkeleton } from '@/app/ui/skeletons';
 
export default async function Page(props: {
  searchParams?: Promise<{
    query?: string;
    page?: string;
  }>;
}) {
  const searchParams = await props.searchParams;
  const query = searchParams?.query || '';
  const currentPage = Number(searchParams?.page) || 1;
 
  return (
    <div className="w-full">
      <div className="flex w-full items-center justify-between">
        <h1 className={`${lusitana.className} text-2xl`}>Invoices</h1>
      </div>
      <div className="mt-4 flex items-center justify-between gap-2 md:mt-8">
        <Search placeholder="Search invoices..." />
        <CreateInvoice />
      </div>
      <Suspense key={query + currentPage} fallback={<InvoicesTableSkeleton />}>
        <Table query={query} currentPage={currentPage} />
      </Suspense>
      <div className="mt-5 flex w-full justify-center">
        {/* <Pagination totalPages={totalPages} /> */}
      </div>
    </div>
  );
}
```

If you navigate to the `<Table>` Component, you'll see that the two props, `query` and `currentPage`, are passed to the `fetchFilteredInvoices()` function which returns the invoices that match the query.

```tsx
// ...
export default async function InvoicesTable({
  query,
  currentPage,
}: {
  query: string;
  currentPage: number;
}) {
  const invoices = await fetchFilteredInvoices(query, currentPage);
  // ...
}
```

With these changes in place, go ahead and test it out. If you search for a term, you'll update the URL, which will send a new request to the server, data will be fetched on the server, and only the invoices that match your query will be returned.

> **When to use the `useSearchParams()` hook vs. the `searchParams` prop?**
> 
> You might have noticed you used two different ways to extract search params. Whether you use one or the other depends on whether you're working on the client or the server.
> * `<Search>` is a Client Component, so you used the `useSearchParams()` hook to access the params from the client.
> * `<Table>` is a Server Component that fetches its own data, so you can pass the `searchParams` prop from the page to the component.
>
> As a general rule, if you want to read the params from the client, use the `useSearchParams()` hook as this avoids having to go back to the server.

## Best practice: Debouncing

**Debouncing** is a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing.

By debouncing, you can reduce the number of requests sent to your database, thus saving resources.

How Debouncing Works:
* **Trigger Event**: When an event that should be debounced (like a keystroke in the search box) occurs, a timer starts.
* **Wait**: If a new event occurs before the timer expires, the timer is reset.
* **Execution**: If the timer reaches the end of its countdown, the debounced function is executed.

### Implementing Debouncing

You've implemented search with Next.js. Now you can optimize it with the debouncing pratice.

You can implement debouncing in a few ways, including manually creating your own debounce function. To keep things simple, we'll use a library called [use-debounce].

Install `use-debounce`:
```sh
pnpm i use-debounce
```

In your `<Search>` Component, import a function called `useDebouncedCallback`:

```tsx
// ...
import { useDebouncedCallback } from 'use-debounce';
 
// Inside the Search Component...
const handleSearch = useDebouncedCallback((term) => {
  console.log(`Searching... ${term}`);
 
  const params = new URLSearchParams(searchParams);
  if (term) {
    params.set('query', term);
  } else {
    params.delete('query');
  }
  replace(`${pathname}?${params.toString()}`);
}, 300);
```

This function will wrap the contents of `handleSearch`, and only run the code after a specific time once the user has stopped typing (300ms).

## Adding pagination

After introducing the search feature, you'll notice the table displays only 6 invoices at a time. This is because the `fetchFilteredInvoices()` function in `data.ts` returns a maximum of 6 invoices per page.

Adding pagination allows users to navigate through the different pages to view all the invoices. Let's see how you can implement pagination using URL params, just like you did with search.

Navigate to the `<Pagination/>` component and you'll notice that it's a Client Component. You don't want to fetch data on the client as this would expose your database secrets (remember, you're not using an API layer). Instead, you can fetch the data on the server, and pass it to the component as a prop.

In `/dashboard/invoices/page.tsx`, import a new function called `fetchInvoicesPages` and pass the query from `searchParams` as an argument:

```tsx
// ...
import { fetchInvoicesPages } from '@/app/lib/data';
 
export default async function Page(
  props: {
    searchParams?: Promise<{
      query?: string;
      page?: string;
    }>;
  }
) {
  const searchParams = await props.searchParams;
  const query = searchParams?.query || '';
  const currentPage = Number(searchParams?.page) || 1;
  const totalPages = await fetchInvoicesPages(query);
 
  return (
    // ...
  );
}
```

`fetchInvoicesPages` returns the total number of pages based on the search query. For example, if there are 12 invoices that match the search query, and each page displays 6 invoices, then the total number of pages would be 2.

Next, pass the `totalPages` prop to the `<Pagination/>` component:

```tsx
// ...
 
export default async function Page(props: {
  searchParams?: Promise<{
    query?: string;
    page?: string;
  }>;
}) {
  const searchParams = await props.searchParams;
  const query = searchParams?.query || '';
  const currentPage = Number(searchParams?.page) || 1;
  const totalPages = await fetchInvoicesPages(query);
 
  return (
    <div className="w-full">
      <div className="flex w-full items-center justify-between">
        <h1 className={`${lusitana.className} text-2xl`}>Invoices</h1>
      </div>
      <div className="mt-4 flex items-center justify-between gap-2 md:mt-8">
        <Search placeholder="Search invoices..." />
        <CreateInvoice />
      </div>
      <Suspense key={query + currentPage} fallback={<InvoicesTableSkeleton />}>
        <Table query={query} currentPage={currentPage} />
      </Suspense>
      <div className="mt-5 flex w-full justify-center">
        <Pagination totalPages={totalPages} />
      </div>
    </div>
  );
}
```

Navigate to the `<Pagination/>` component and import the `usePathname` and `useSearchParams` hooks. We will use this to get the current page and set the new page. Make sure to also uncomment the code in this component. Your application will break temporarily as you haven't implemented the `<Pagination/>` logic yet. Let's do that now!

```tsx
'use client';
 
import { ArrowLeftIcon, ArrowRightIcon } from '@heroicons/react/24/outline';
import clsx from 'clsx';
import Link from 'next/link';
import { generatePagination } from '@/app/lib/utils';
import { usePathname, useSearchParams } from 'next/navigation';
 
export default function Pagination({ totalPages }: { totalPages: number }) {
  const pathname = usePathname();
  const searchParams = useSearchParams();
  const currentPage = Number(searchParams.get('page')) || 1;
 
  // ...
}
```

Next, create a new function inside the `<Pagination>` Component called `createPageURL`. Similarly to the search, you'll use `URLSearchParams` to set the new page number, and `pathName` to create the URL string.

```tsx
'use client';
 
import { ArrowLeftIcon, ArrowRightIcon } from '@heroicons/react/24/outline';
import clsx from 'clsx';
import Link from 'next/link';
import { generatePagination } from '@/app/lib/utils';
import { usePathname, useSearchParams } from 'next/navigation';
 
export default function Pagination({ totalPages }: { totalPages: number }) {
  const pathname = usePathname();
  const searchParams = useSearchParams();
  const currentPage = Number(searchParams.get('page')) || 1;
 
  const createPageURL = (pageNumber: number | string) => {
    const params = new URLSearchParams(searchParams);
    params.set('page', pageNumber.toString());
    return `${pathname}?${params.toString()}`;
  };
 
  // ...
}
```

Here's a breakdown of what's happening:
* `createPageURL` creates an instance of the current search parameters.
* Then, it updates the "page" parameter to the provided page number.
* Finally, it constructs the full URL using the pathname and updated search parameters.

The rest of the `<Pagination>` component deals with styling and different states (first, last, active, disabled, etc). We won't go into detail for this course, but feel free to look through the code to see where `createPageURL` is being called.

Finally, when the user types a new search query, you want to reset the page number to 1. You can do this by updating the `handleSearch` function in your `<Search>` component:

```tsx
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { usePathname, useRouter, useSearchParams } from 'next/navigation';
import { useDebouncedCallback } from 'use-debounce';
 
export default function Search({ placeholder }: { placeholder: string }) {
  const searchParams = useSearchParams();
  const { replace } = useRouter();
  const pathname = usePathname();
 
  const handleSearch = useDebouncedCallback((term) => {
    const params = new URLSearchParams(searchParams);
    params.set('page', '1');
    if (term) {
      params.set('query', term);
    } else {
      params.delete('query');
    }
    replace(`${pathname}?${params.toString()}`);
  }, 300);
 
```



# Chapter 12 - Mutating Data

Here are the topics we’ll cover
* What React Server Actions are and how to use them to mutate data.
* How to work with forms and Server Components.
* Best practices for working with the native `FormData` object, including type validation.
* How to revalidate the client cache using the `revalidatePath` API.
* How to create dynamic route segments with specific IDs.

## What are Server Actions?

React Server Actions allow you to run asynchronous code directly on the server. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

Security is a top priority for web applications, as they can be vulnerable to various threats. This is where Server Actions come in. They include features like encrypted closures, strict input checks, error message hashing, host restrictions, and more — all working together to significantly enhance your application security.

## Using forms with Server Actions

In React, you can use the `action` attribute in the `<form>` element to invoke actions. The action will automatically receive the native [FormData] object, containing the captured data.

For example:

```tsx
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';
 
    // Logic to mutate data...
  }
 
  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

## Next.js with Server Actions

Server Actions are also deeply integrated with Next.js [caching]. When a form is submitted through a Server Action, not only can you use the action to mutate data, but you can also revalidate the associated cache using APIs like `revalidatePath` and `revalidateTag`.

## Creating an invoice

Here are the steps you'll take to create a new invoice:
1. Create a form to capture the user's input.
2. Create a Server Action and invoke it from the form.
3. Inside your Server Action, extract the data from the formData object.
4. Validate and prepare the data to be inserted into your database.
5. Insert the data and handle any errors.
6. Revalidate the cache and redirect the user back to invoices page.

### 1. Create a new route and form

To start, inside the `/invoices` folder, add a new route segment called `/create` with a `page.tsx` file:

![Invoices folder with a nested create folder, and a page.tsx file inside it](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fcreate-invoice-route.png&w=1920&q=75)

You'll be using this route to create new invoices. Inside your `page.tsx` file, paste the following code, then spend some time studying it:

```tsx
import Form from '@/app/ui/invoices/create-form';
import Breadcrumbs from '@/app/ui/invoices/breadcrumbs';
import { fetchCustomers } from '@/app/lib/data';
 
export default async function Page() {
  const customers = await fetchCustomers();
 
  return (
    <main>
      <Breadcrumbs
        breadcrumbs={[
          { label: 'Invoices', href: '/dashboard/invoices' },
          {
            label: 'Create Invoice',
            href: '/dashboard/invoices/create',
            active: true,
          },
        ]}
      />
      <Form customers={customers} />
    </main>
  );
}
```

Your page is a Server Component that fetches `customers` and passes it to the `<Form>` component. To save time, we've already created the `<Form>` component for you.

Navigate to the `<Form>` component, and you'll see that the form:
* Has one `<select>` (dropdown) element with a list of **customers**.
* Has one `<input>` element for the **amount** with `type="number"`.
* Has two `<input>` elements for the status with `type="radio"`.
* Has one button with `type="submit"`.

On http://localhost:3000/dashboard/invoices/create, you should see the following UI:

![Create invoices page with breadcrumbs and form](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fcreate-invoice-page.png&w=1080&q=75)

### 2. Create a Server Action

Great, now let's create a Server Action that is going to be called when the form is submitted.

Navigate to your `lib/` directory and create a new file named `actions.ts`. At the top of this file, add the React [use server] directive:

```ts
'use server';
```

 adding the `'use server'`, you mark all the exported functions within the file as Server Actions. These server functions can then be imported and used in Client and Server components. Any functions included in this file that are not used will be automatically removed from the final application bundle.

You can also write Server Actions directly inside Server Components by adding `"use server"` inside the action. But for this course, we'll keep them all organized in a separate file. We recommend having a separate file for your actions.

In your `actions.ts` file, create a new async function that accepts `formData`:

```ts
'use server';
 
export async function createInvoice(formData: FormData) {}
```

Then, in your `<Form>` component, import the `createInvoice` from your `actions.ts` file. Add a `action` attribute to the `<form>` element, and call the `createInvoice` action.

```tsx
import { CustomerField } from '@/app/lib/definitions';
import Link from 'next/link';
import {
  CheckIcon,
  ClockIcon,
  CurrencyDollarIcon,
  UserCircleIcon,
} from '@heroicons/react/24/outline';
import { Button } from '@/app/ui/button';
import { createInvoice } from '@/app/lib/actions';
 
export default function Form({
  customers,
}: {
  customers: CustomerField[];
}) {
  return (
    <form action={createInvoice}>
      // ...
  )
}
```

>**Good to know**: In HTML, you'd pass a URL to the `action` attribute. This URL would be the destination where your form data should be submitted (usually an API endpoint).
>
>However, in React, the `action` attribute is considered a special prop - meaning React builds on top of it to allow actions to be invoked.
>
>Behind the scenes, Server Actions create a `POST` API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.

### 3. Extract the data from `formData`

Back in your `actions.ts` file, you'll need to extract the values of `formData`, there are a [couple of methods] you can use. For this example, let's use the [.get(name)] method.

```ts
'use server';
 
export async function createInvoice(formData: FormData) {
  const rawFormData = {
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  };
  // Test it out:
  console.log(rawFormData);
}
```

> **Tip**: If you're working with forms that have many fields, you may want to consider using the [entries()] method with JavaScript's [Object.fromEntries()].

To check everything is connected correctly, try out the form. After submitting, you should see the data you just entered into the form logged in your **terminal** (not the browser).

Now that your data is in the shape of an object, it'll be much easier to work with.

### 4. Validate and prepare the data

Before sending the form data to your database, you want to ensure it's in the correct format and with the correct types. If you remember from earlier in the course, your invoices table expects data in the following format:

```ts
export type Invoice = {
  id: string; // Will be created on the database
  customer_id: string;
  amount: number; // Stored in cents
  status: 'pending' | 'paid';
  date: string;
};
```

So far, you only have the `customer_id`, `amount`, and `status` from the form.

#### Type validation and coercion

It's important to validate that the data from your form aligns with the expected types in your database. For instance, if you add a console.log inside your action:

```ts
console.log(typeof rawFormData.amount);
```

You'll notice that `amount` is of type `string` and not `number`. This is because `input` elements with `type="number"` actually return a string, not a number!

To handle type validation, you have a few options. While you can manually validate types, using a type validation library can save you time and effort. For your example, we'll use [Zod], a TypeScript-first validation library that can simplify this task for you.

In your `actions.ts` file, import Zod and define a schema that matches the shape of your form object. This schema will validate the `formData` before saving it to a database.

```ts
'use server';
 
import { z } from 'zod';
 
const FormSchema = z.object({
  id: z.string(),
  customerId: z.string(),
  amount: z.coerce.number(),
  status: z.enum(['pending', 'paid']),
  date: z.string(),
});
 
const CreateInvoice = FormSchema.omit({ id: true, date: true });
 
export async function createInvoice(formData: FormData) {
  // ...
}
```

The `amount` field is specifically set to coerce (change) from a string to a number while also validating its type.

You can then pass your `rawFormData` to `CreateInvoice` to validate the types:

```ts
// ...
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
}
```

#### Storing values in cents

It's usually good practice to store monetary values in cents in your database to eliminate JavaScript floating-point errors and ensure greater accuracy.

Let's convert the amount into cents:

```ts
// ...
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
  const amountInCents = amount * 100;
}
```

#### Creating new dates

Finally, let's create a new date with the format "YYYY-MM-DD" for the invoice's creation date:

```ts
// ...
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
  const amountInCents = amount * 100;
  const date = new Date().toISOString().split('T')[0];
}
```

### 5. Inserting the data into your database

Now that you have all the values you need for your database, you can create an SQL query to insert the new invoice into your database and pass in the variables:

```ts
import { z } from 'zod';
import postgres from 'postgres';
 
const sql = postgres(process.env.POSTGRES_URL!, { ssl: 'require' });
 
// ...
 
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
  const amountInCents = amount * 100;
  const date = new Date().toISOString().split('T')[0];
 
  await sql`
    INSERT INTO invoices (customer_id, amount, status, date)
    VALUES (${customerId}, ${amountInCents}, ${status}, ${date})
  `;
}
```

Right now, we're not handling any errors. We'll talk about this in the next chapter. For now, let's move on to the next step.

### 6. Revalidate and redirect

Next.js has a client-side router cache that stores the route segments in the user's browser for a time. Along with [prefetching], this cache ensures that users can quickly navigate between routes while reducing the number of requests made to the server.

Since you're updating the data displayed in the invoices route, you want to clear this cache and trigger a new request to the server. You can do this with the [revalidatePath] function from Next.js:

```ts
'use server';
 
import { z } from 'zod';
import { revalidatePath } from 'next/cache';
import postgres from 'postgres';
 
// ...
 
export async function createInvoice(formData: FormData) {
  // ...

  revalidatePath('/dashboard/invoices');
}
```

Once the database has been updated, the `/dashboard/invoices` path will be revalidated, and fresh data will be fetched from the server.

At this point, you also want to redirect the user back to the `/dashboard/invoices` page. You can do this with the [redirect] function from Next.js:

```ts
'use server';
 
import { z } from 'zod';
import { revalidatePath } from 'next/cache';
import { redirect } from 'next/navigation';
import postgres from 'postgres';
 
// ...
 
export async function createInvoice(formData: FormData) {
  // ...
 
  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}
```

Congratulations! You've just implemented your first Server Action. Test it out by adding a new invoice, if everything is working correctly:
* You should be redirected to the `/dashboard/invoices` route on submission.
* You should see the new invoice at the top of the table.

## Updating an invoice

The updating invoice form is similar to the create an invoice form, except you'll need to pass the invoice `id` to update the record in your database. Let's see how you can get and pass the invoice `id`.

These are the steps you'll take to update an invoice:
1. Create a new dynamic route segment with the invoice id.
2. Read the invoice id from the page params.
3. Fetch the specific invoice from your database.
4. Pre-populate the form with the invoice data.
5. Update the invoice data in your database.

### 1. Create a Dynamic Route Segment with the invoice id

Next.js allows you to create [Dynamic Route Segments] when you don't know the exact segment name and want to create routes based on data. This could be blog post titles, product pages, etc. You can create dynamic route segments by wrapping a folder's name in square brackets. For example, `[id]`, `[post]` or `[slug]`.

In your `/invoices` folder, create a new dynamic route called `[id]`, then a new route called `edit` with a `page.tsx` file. Your file structure should look like this:

![Invoices folder with a nested [id] folder, and an edit folder inside it](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fedit-invoice-route.png&w=1920&q=75)

In your `<Table>` component, notice there's a `<UpdateInvoice />` button that receives the invoice's `id` from the table records.

```tsx
export default async function InvoicesTable({
  query,
  currentPage,
}: {
  query: string;
  currentPage: number;
}) {
  return (
    // ...
    <td className="flex justify-end gap-2 whitespace-nowrap px-6 py-4 text-sm">
      <UpdateInvoice id={invoice.id} />
      <DeleteInvoice id={invoice.id} />
    </td>
    // ...
  );
}
```

```tsx
import { PencilIcon, PlusIcon, TrashIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
 
// ...

 Navigate to your `<UpdateInvoice />` component, and update the `href` of the `Link` to accept the `id` prop. You can use template literals to link to a dynamic route segment:

export function UpdateInvoice({ id }: { id: string }) {
  return (
    <Link
      href={`/dashboard/invoices/${id}/edit`}
      className="rounded-md border p-2 hover:bg-gray-100"
    >
      <PencilIcon className="w-5" />
    </Link>
  );
}
```

### 2. Read the invoice `id` from page `params`

Back on your <Page> component, paste the following code:

```tsx
import Form from '@/app/ui/invoices/edit-form';
import Breadcrumbs from '@/app/ui/invoices/breadcrumbs';
import { fetchCustomers } from '@/app/lib/data';
 
export default async function Page() {
  return (
    <main>
      <Breadcrumbs
        breadcrumbs={[
          { label: 'Invoices', href: '/dashboard/invoices' },
          {
            label: 'Edit Invoice',
            href: `/dashboard/invoices/${id}/edit`,
            active: true,
          },
        ]}
      />
      <Form invoice={invoice} customers={customers} />
    </main>
  );
}
```

Notice how it's similar to your `/create` invoice page, except it imports a different form (from the `edit-form.tsx` file). This form should be **pre-populated** with a `defaultValue` for the customer's name, invoice amount, and status. To pre-populate the form fields, you need to fetch the specific invoice using `id`.

In addition to `searchParams`, page components also accept a prop called `params` which you can use to access the `id`. Update your `<Page>` component to receive the prop:

```tsx
import Form from '@/app/ui/invoices/edit-form';
import Breadcrumbs from '@/app/ui/invoices/breadcrumbs';
import { fetchCustomers } from '@/app/lib/data';
 
export default async function Page(props: { params: Promise<{ id: string }> }) {
  const params = await props.params;
  const id = params.id;
  // ...
}
```

### 3. Fetch the specific invoice

Then:
* Import a new function called `fetchInvoiceById` and pass the `id` as an argument.
* Import `fetchCustomers` to fetch the customer names for the dropdown.

You can use `Promise.all` to fetch both the invoice and customers in parallel:

```tsx
import Form from '@/app/ui/invoices/edit-form';
import Breadcrumbs from '@/app/ui/invoices/breadcrumbs';
import { fetchInvoiceById, fetchCustomers } from '@/app/lib/data';
 
export default async function Page(props: { params: Promise<{ id: string }> }) {
  const params = await props.params;
  const id = params.id;
  const [invoice, customers] = await Promise.all([
    fetchInvoiceById(id),
    fetchCustomers(),
  ]);
  // ...
}
```

You will see a temporary TypeScript error for the `invoice` prop in your terminal because `invoice` could be potentially `undefined`. Don't worry about it for now, you'll resolve it in the next chapter when you add error handling.

Great! Now, test that everything is wired correctly. Visit http://localhost:3000/dashboard/invoices and click on the Pencil icon to edit an invoice. After navigation, you should see a form that is pre-populated with the invoice details:

![Edit invoices page with breadcrumbs and form](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fedit-invoice-page.png&w=1080&q=75)

The URL should also be updated with an `id` as follows: `http://localhost:3000/dashboard/invoice/uuid/edit`

> **UUIDs vs. Auto-incrementing Keys**
> 
> We use UUIDs instead of incrementing keys (e.g., 1, 2, 3, etc.). This makes the URL longer; however, UUIDs eliminate the risk of ID collision, are globally unique, and reduce the risk of enumeration attacks - making them ideal for large databases.
> 
> However, if you prefer cleaner URLs, you might prefer to use auto-incrementing keys.

### 4. Pass the `id` to the Server Action

Lastly, you want to pass the `id` to the Server Action so you can update the right record in your database. You **cannot** pass the `id` as an argument like so:

```tsx
// Passing an id as argument won't work
<form action={updateInvoice(id)}>
```

Instead, you can pass `id` to the Server Action using JS `bind`. This will ensure that any values passed to the Server Action are encoded.

```tsx
// ...
import { updateInvoice } from '@/app/lib/actions';
 
export default function EditInvoiceForm({
  invoice,
  customers,
}: {
  invoice: InvoiceForm;
  customers: CustomerField[];
}) {
  const updateInvoiceWithId = updateInvoice.bind(null, invoice.id);
 
  return <form action={updateInvoiceWithId}>{/* ... */}</form>;
}
```

> **Note**: Using a hidden input field in your form also works (e.g. `<input type="hidden" name="id" value={invoice.id} />`). However, the values will appear as full text in the HTML source, which is not ideal for sensitive data.

Then, in your actions.ts file, create a new action, updateInvoice:

```ts
// Use Zod to update the expected types
const UpdateInvoice = FormSchema.omit({ id: true, date: true });
 
// ...
 
export async function updateInvoice(id: string, formData: FormData) {
  const { customerId, amount, status } = UpdateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
 
  const amountInCents = amount * 100;
 
  await sql`
    UPDATE invoices
    SET customer_id = ${customerId}, amount = ${amountInCents}, status = ${status}
    WHERE id = ${id}
  `;
 
  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}
```

Similarly to the `createInvoice` action, here you are:
1. Extracting the data from `formData`.
2. Validating the types with Zod.
3. Converting the amount to cents.
4. Passing the variables to your SQL query.
5. Calling `revalidatePath` to clear the client cache and make a new server request.
6. Calling `redirect` to redirect the user to the invoice's page.

Test it out by editing an invoice. After submitting the form, you should be redirected to the invoices page, and the invoice should be updated.

## Deleting an invoice

To delete an invoice using a Server Action, wrap the delete button in a `<form>` element and pass the `id` to the Server Action using `bind`:

```tsx
import { deleteInvoice } from '@/app/lib/actions';
 
// ...
 
export function DeleteInvoice({ id }: { id: string }) {
  const deleteInvoiceWithId = deleteInvoice.bind(null, id);
 
  return (
    <form action={deleteInvoiceWithId}>
      <button type="submit" className="rounded-md border p-2 hover:bg-gray-100">
        <span className="sr-only">Delete</span>
        <TrashIcon className="w-4" />
      </button>
    </form>
  );
}
```

Inside your actions.ts file, create a new action called deleteInvoice.

```ts
export async function deleteInvoice(id: string) {
  await sql`DELETE FROM invoices WHERE id = ${id}`;
  revalidatePath('/dashboard/invoices');
}
```

Since this action is being called in the `/dashboard/invoices` path, you don't need to call `redirect`. Calling `revalidatePath` will trigger a new server request and re-render the table.

## Further reading

In this chapter, you learned how to use Server Actions to mutate data. You also learned how to use the `revalidatePath` API to revalidate the Next.js cache and `redirect` to redirect the user to a new page.

You can also read more about [security with Server Actions] for additional learning.



# Chapter 13 - Handling Errors

Let's see how you can handle errors gracefully using JavaScript's try/catch statements and Next.js APIs for uncaught exceptions.

Here are the topics we’ll cover:
* How to use the special `error.tsx` file to catch errors in your route segments, and show a fallback UI to the user.
* How to use the `notFound` function and `not-found` file to handle 404 errors (for resources that don’t exist).

## Adding try/catch to Server Actions

First, let's add JavaScript's try/catch statements to your Server Actions to allow you to handle errors gracefully.

```ts
export async function createInvoice(formData: FormData) {
  
  // ...
 
  try {
    await sql`
      INSERT INTO invoices (customer_id, amount, status, date)
      VALUES (${customerId}, ${amountInCents}, ${status}, ${date})
    `;
  } catch (error) {
    // We'll log the error to the console for now
    console.error(error);
  }
 
  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}
```

Note how `redirect` is being called outside of the `try/catch` block. This is because **`redirect` works by throwing an error**, which would be caught by the catch block. To avoid this, you can call `redirect` after try/catch. redirect would only be reachable if `try` is successful.

We're handling these errors by catching the database issue, and returning a helpful message from our Server Action.

When going to production, you want to more gracefully show a message to the user when something unexpected happens.

This is where Next.js [error.tsx] file comes in. Ensure that you remove this manually added error after testing and before moving onto the next section.


## Handling all errors with error.tsx

The `error.tsx` file can be used to define a UI boundary for a route segment. It serves as a **catch-all** for unexpected errors and allows you to display a fallback UI to your users.

Inside your `/dashboard/invoices` folder, create a new file called `error.tsx` and paste the following code:

```tsx
'use client';
 
import { useEffect } from 'react';
 
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    // Optionally log the error to an error reporting service
    console.error(error);
  }, [error]);
 
  return (
    <main className="flex h-full flex-col items-center justify-center">
      <h2 className="text-center">Something went wrong!</h2>
      <button
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
        onClick={
          // Attempt to recover by trying to re-render the invoices route
          () => reset()
        }
      >
        Try again
      </button>
    </main>
  );
}
```

There are a few things you'll notice about the code above:
* **"use client"** - `error.tsx` needs to be a Client Component.
* It accepts two props:
  1. `error`: This object is an instance of JavaScript's native [Error] object.
  2. `reset`: This is a function to reset the error boundary. When executed, the function will try to re-render the route segment.

When an error occurs, you should see the following UI:

![The error.tsx file showing the props it accepts](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Ferror-page.png&w=1080&q=75)

## Handling 404 errors with the `notFound` function

Another way you can handle errors gracefully is by using the `notFound` function. While `error.tsx` is useful for catching uncaught exceptions, `notFound` can be used when you try to fetch a resource that doesn't exist.

For example, visit http://localhost:3000/dashboard/invoices/2e94d1ed-d220-449f-9f11-f0bbceed9645/edit.

This is a fake UUID that doesn't exist in your database.

You'll immediately see `error.tsx` kicks in because this is a child route of `/invoices` where `error.tsx` is defined.

However, if you want to be more specific, you can show a 404 error to tell the user the resource they're trying to access hasn't been found.

Navigate to `/dashboard/invoices/[id]/edit/page.tsx`, and import `{ notFound }` from `'next/navigation'`.

Then, you can use a conditional to invoke `notFound` if the invoice doesn't exist:

```tsx
import { fetchInvoiceById, fetchCustomers } from '@/app/lib/data';
import { notFound } from 'next/navigation';
 
export default async function Page(props: { params: Promise<{ id: string }> }) {
  const params = await props.params;
  const id = params.id;
  const [invoice, customers] = await Promise.all([
    fetchInvoiceById(id),
    fetchCustomers(),
  ]);
 
  if (!invoice) {
    notFound();
  }
 
  // ...
}
```

Then, to show error UI to the user, create a `not-found.tsx` file inside the `/edit` folder.

![The not-found.tsx file inside the edit folder](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fnot-found-file.png&w=1920&q=75)

Inside the `not-found.tsx` file, paste the following the code:

```tsx
import Link from 'next/link';
import { FaceFrownIcon } from '@heroicons/react/24/outline';

export default function NotFound() {
  return (
    <main className="flex h-full flex-col items-center justify-center gap-2">
      <FaceFrownIcon className="w-10 text-gray-400" />
      <h2 className="text-xl font-semibold">404 Not Found</h2>
      <p>Could not find the requested invoice.</p>
      <Link
        href="/dashboard/invoices"
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
      >
        Go Back
      </Link>
    </main>
  );
}
```

Refresh the route, and you should now see the following UI:

![404 Not Found Page](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2F404-not-found-page.png&w=1080&q=75)

That's something to keep in mind, `notFound` will take precedence over `error.tsx`, so you can reach out for it when you want to handle more specific errors!

## Further reading

To learn more about error handling in Next.js, check out the following documentation:

* [Error Handling]
* [error.js API Reference]
* [notFound() API Reference]
* [not-found.js API Reference]




# Chapter 14 - Improving Accessibility 

Here are the topics we’ll cover
* How to use `eslint-plugin-jsx-a11y` with Next.js to implement accessibility best practices.
* How to implement server-side form validation.
* How to use the React useActionState hook to handle form errors, and display them to the user.


## What is accessibility?

Accessibility refers to designing and implementing web applications that everyone can use, including those with disabilities. 

It's a vast topic that covers many areas, such as keyboard navigation, semantic HTML, images, colors, videos, etc.

While we won't go in-depth into accessibility in this course, we'll discuss the accessibility features available in Next.js and some common practices to make your applications more accessible.

> If you'd like to learn more about accessibility, we recommend the [Learn Accessibility] course by [web.dev].

## Using the ESLint accessibility plugin in Next.js

Next.js includes the [eslint-plugin-jsx-a11y] plugin in its ESLint config to help catch accessibility issues early. For example, this plugin warns if you have images without `alt` text, use the `aria-*` and `role` attributes incorrectly, and more.

Optionally, if you would like to try this out, add `next lint` as a script in your `package.json` file:

```json
"scripts": {
    "build": "next build",
    "dev": "next dev",
    "start": "next start",
    "lint": "next lint"
},
```

Then run pnpm lint in your terminal:

```sh
pnpm lint
```

Now, you should see the following output:

```sh
✔ No ESLint warnings or errors
```

However, what would happen if you had an image without alt text? Let's find out!

```sh
./app/ui/invoices/table.tsx
45:25  Warning: Image elements must have an alt prop,
either with meaningful text, or an empty string for decorative images. jsx-a11y/alt-text
```

While adding and configuring a linter is not a required step, it can be helpful to catch accessibility issues in your development process.

## Improving form accessibility

There are three things we're already doing to improve accessibility in our forms:
1. **Semantic HTML**: Using semantic elements (`<input>`, `<option>`, etc) instead of `<div>`. This allows assistive technologies (AT) to focus on the input elements and provide appropriate contextual information to the user, making the form easier to navigate and understand.
2. **Labelling**: Including `<label>` and the `htmlFor` attribute ensures that each form field has a descriptive text label. This improves AT support by providing context and also enhances usability by allowing users to click on the label to focus on the corresponding input field.
3. **Focus Outline**: The fields are properly styled to show an outline when they are in focus. This is critical for accessibility as it visually indicates the active element on the page, helping both keyboard and screen reader users to understand where they are on the form. You can verify this by pressing `tab`.

These practices lay a good foundation for making your forms more accessible to many users. However, they don't address **form validation** and **errors**.

## Form validation

Go to http://localhost:3000/dashboard/invoices/create, and submit an empty form. What happens?

You get an error! This is because you're sending empty form values to your Server Action. You can prevent this by validating your form on the client or the server.

### Client-Side validation

There are a couple of ways you can validate forms on the client. The simplest would be to rely on the form validation provided by the browser by adding the `required` attribute to the `<input>` and `<select>` elements in your forms. For example:

```tsx
<input
  id="amount"
  name="amount"
  type="number"
  placeholder="Enter USD amount"
  className="peer block w-full rounded-md border border-gray-200 py-2 pl-10 text-sm outline-2 placeholder:text-gray-500"
  required
/>
```

The browser will display a warning if you try to submit a form with empty values.

This approach is generally okay because some ATs support browser validation.

### Server-Side validation

By validating forms on the server, you can:
* Ensure your data is in the expected format before sending it to your database.
* Reduce the risk of malicious users bypassing client-side validation.
* Have one source of truth for what is considered valid data.

See the full guide [here].



# Chapter 15 - Adding Authentication

In this chapter, you'll be adding authentication to your dashboard.

Here are the topics we’ll cover:
* What is authentication.
* How to add authentication to your app using NextAuth.js.
* How to use Middleware to redirect users and protect your routes.
* How to use React's `useActionState` to handle pending states and form errors.

## What is authentication?

It's how a system checks if the user is who they say they are.

A secure website often uses multiple ways to check a user's identity. For instance, after entering your username and password, the site may send a verification code to your device or use an external app like Google Authenticator (2FA).

## Authentication vs. Authorization

In web development, authentication and authorization serve different roles:
* **Authentication** is about making sure the user is who they say they are. You're proving your identity with something you have like a username and password.
* **Authorization** is the next step. Once a user's identity is confirmed, authorization decides what parts of the application they are allowed to use.

So, authentication checks who you are, and authorization determines what you can do or access in the application.

## Creating the login route

Start by creating a new route in your application called `/login` and paste the following code:

```tsx
import AcmeLogo from '@/app/ui/acme-logo';
import LoginForm from '@/app/ui/login-form';
import { Suspense } from 'react';
 
export default function LoginPage() {
  return (
    <main className="flex items-center justify-center md:h-screen">
      <div className="relative mx-auto flex w-full max-w-[400px] flex-col space-y-2.5 p-4 md:-mt-32">
        <div className="flex h-20 w-full items-end rounded-lg bg-blue-500 p-3 md:h-36">
          <div className="w-32 text-white md:w-36">
            <AcmeLogo />
          </div>
        </div>
        <Suspense>
          <LoginForm />
        </Suspense>
      </div>
    </main>
  );
}
```

You'll notice the page imports `<LoginForm />`, which you'll update later in the chapter. This component is wrapped with React `<Suspense>` because it will access information from the incoming request (URL search params).

## NextAuth.js

[NextAuth.js] abstracts away much of the complexity involved in managing sessions, sign-in and sign-out, and other aspects of authentication. 

While you can manually implement these features, the process can be time-consuming and error-prone. 

NextAuth.js simplifies the process, providing a unified solution for auth in Next.js applications.

We will be using NextAuth.js to add authentication to your application. 

[See how to setting up NextAuth.js].

## Protecting your routes with Next.js Middleware

Next, add the logic to protect your routes. This will prevent users from accessing the dashboard pages unless they are logged in.

The advantage of employing Middleware for this task is that the protected routes will not even start rendering until the Middleware verifies the authentication, enhancing both the security and performance of your application.

[See how to setting up Next.js Middleware].

## Password hashing

It's good practice to hash passwords before storing them in a database. Hashing converts a password into a fixed-length string of characters, which appears random, providing a layer of security even if the user's data is exposed.

We will use `bcryptjs` again later in this chapter to compare that the password entered by the user matches the one in the database. 

[See how to hashing a password].

## Adding the Credentials provider

[See how to add the Credentials provider].

## Adding the sign in functionality

[See how to add the sign in funcionality].

## Updating the login form

[See how to update the login form].

## Adding the logout functionality

[See how to add the logout functionality].

## Try it out

You should be able to log in and out of your application using the following credentials:
* Email: user@nextmail.com
* Password: 123456



# Chapter 16 - Adding Metadata

Metadata is crucial for SEO and shareability. In this chapter, we'll discuss how you can add metadata to your Next.js application.

Here are the topics we’ll cover:
* What metadata is.
* Types of metadata.
* How to add an Open Graph image using metadata.
* How to add a favicon using metadata.

## What is metadata?

In web development, metadata provides additional details about a webpage. 

Metadata is not visible to the users visiting the page. Instead, it works behind the scenes, embedded within the page's HTML, usually within the <head> element. 

This information is crucial for search engines and other systems that need to understand your webpage's content better.

## Why is metadata important?

Metadata plays a significant role in enhancing a webpage's SEO, making it more accessible and understandable for search engines and social media platforms. 

Proper metadata helps search engines effectively index webpages, improving their ranking in search results. 

Additionally, metadata like Open Graph improves the appearance of shared links on social media, making the content more appealing and informative for users.

## Types of metadata

There are various types of metadata, each serving a unique purpose. Some common types include:

**Title Metadata**: Responsible for the title of a webpage that is displayed on the browser tab. It's crucial for SEO as it helps search engines understand what the webpage is about.

```html
<title>Page Title</title>
```

**Description Metadata**: This metadata provides a brief overview of the webpage content and is often displayed in search engine results.

```html
<meta name="description" content="A brief description of the page content." />
```

**Keyword Metadata**: This metadata includes the keywords related to the webpage content, helping search engines index the page.

```html
<meta name="keywords" content="keyword1, keyword2, keyword3" />
```

**Open Graph Metadata**: This metadata enhances the way a webpage is represented when shared on social media platforms, providing information such as the title, description, and preview image.

```html
<meta property="og:title" content="Title Here" />
<meta property="og:description" content="Description Here" />
<meta property="og:image" content="image_url_here" />
```

**Favicon Metadata**: This metadata links the favicon (a small icon) to the webpage, displayed in the browser's address bar or tab.

```html
<link rel="icon" href="path/to/favicon.ico" />
```

## Adding metadata

Next.js has a Metadata API that can be used to define your application metadata. 

There are two ways you can add metadata to your application:

1. **Config-based**: Export a [static metadata object] or a dynamic [generateMetadata function] in a `layout.js` or `page.js` file.

2. **File-based**: Next.js has a range of special files that are specifically used for metadata purposes:
  * `favicon.ico`, `apple-icon.jpg`, and `icon.jpg`: Utilized for favicons and icons
  * `opengraph-image.jpg` and `twitter-image.jpg`: Employed for social media images
  * `robots.txt`: Provides instructions for search engine crawling
  * `sitemap.xml`: Offers information about the website's structure

You have the flexibility to use these files for static metadata, or you can generate them programmatically within your project.

With both these options, Next.js will automatically generate the relevant `<head>` elements for your pages.

## Favicon and Open Graph image

In your `/public` folder, you'll notice you have two images: `favicon.ico` and `opengraph-image.jpg`.

Move these images to the root of your `/app` folder.

After doing this, Next.js will automatically identify and use these files as your favicon and OG image. You can verify this by checking the `<head>` element of your application in dev tools.

> Good to know: You can also create dynamic OG images using the [ImageResponse] constructor.

## Page title and descriptions

You can also include a [metadata object] from any `layout.js` or `page.js` file to add additional page information like title and description. Any metadata in `layout.js` will be inherited by all pages that use it.

In your root layout, create a new `metadata` object with the following fields:

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Acme Dashboard',
  description: 'The official Next.js Course Dashboard, built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};
 
export default function RootLayout() {
  // ...
}
```

Next.js will automatically add the title and metadata to your application.

But what if you want to add a custom title for a specific page? You can do this by adding a `metadata` object to the page itself. Metadata in nested pages will override the metadata in the parent.

For example, in the `/dashboard/invoices` page, you can update the page title:

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Invoices | Acme Dashboard',
};
```

This works, but we are repeating the title of the application in every page. If something changes, like the company name, you'd have to update it on every page.

Instead, you can use the `title.template` field in the `metadata` object to define a template for your page titles. This template can include the page title, and any other information you want to include.

In your root layout, update the `metadata` object to include a template:

```tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    template: '%s | Acme Dashboard',
    default: 'Acme Dashboard',
  },
  description: 'The official Next.js Learn Dashboard built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};
```

The `%s` in the template will be replaced with the specific page title.

Now, in your `/dashboard/invoices` page, you can add the page title:

```tsx
export const metadata: Metadata = {
  title: 'Invoices',
};
```

Navigate to the `/dashboard/invoices` page and check the `<head>` element. You should see the page title is now `Invoices | Acme Dashboard`.

## Practice: Adding metadata

Now that you've learned about metadata, practice by adding titles to your other pages:
1. `/login` page.
2. `/dashboard/` page.
3. `/dashboard/customers` page.
4. `/dashboard/invoices/create` page.
5. `/dashboard/invoices/[id]/edit` page.

The Next.js Metadata API is powerful and flexible, giving you full control over your application's metadata. Here, we've shown you how to add some basic metadata, but you can add multiple fields, including `keywords`, `robots`, `canonical`, and more. Feel free to explore the [generate-metadata documentation], and add any additional metadata you want to your application.



# Chapter 17 - Next Steps

Congratulations! You've completed the Next.js dashboard course where you learned about the main features of Next.js and best practices for building web applications.

But this is just the beginning—Next.js has many other features. It's designed to help you build small side projects, your next startup idea, or even large-scale applications with your team.

Here are some resources to continue exploring Next.js:

* [Next.js Documentation]
* [Next.js Templates]:
  * [Admin Dashboard Template]
  * [Next.js Commerce]
  * [Blog Starter Kit]
  * [AI Chatbot]
  * [Image Gallery Starter]
* [Next.js Repository]
* [Vercel YouTube]
* [Vercel Reddit]



[//]: # (Links)

[pnpm]: <https://pnpm.io/>

[cd]: <https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line#basic_built-in_terminal_commands>

[create-next-app]: <https://nextjs.org/docs/app/api-reference/create-next-app>

[starter example]: <https://github.com/vercel/next-learn/tree/main/dashboard/starter-example>

[mockAPI]: <https://mockapi.io/>

[root layout]: <https://nextjs.org/docs/app/api-reference/file-conventions/layout#root-layouts>

[Tailwind]: <https://tailwindcss.com/>

[utility classes]: <https://tailwindcss.com/docs/utility-first>

[CSS Modules]: <https://nextjs.org/docs/basic-features/built-in-css-support>

[clsx]: <https://www.npmjs.com/package/clsx>

[documentation]: <https://github.com/lukeed/clsx>

[styled-jsx]: <https://github.com/vercel/styled-jsx>

[styled-components]: <https://github.com/vercel/next.js/tree/canary/examples/with-styled-components>

[emotion]: <https://github.com/vercel/next.js/tree/canary/examples/with-emotion>

[CSS documentation]: <https://nextjs.org/docs/app/building-your-application/styling>

[Cumulative Layout Shift]: <https://vercel.com/blog/how-core-web-vitals-affect-seo>

[subset]: <https://fonts.google.com/knowledge/glossary/subsetting>

[antialiased]: <https://tailwindcss.com/docs/font-smoothing>

[/public]: <https://nextjs.org/docs/app/building-your-application/optimizing/static-assets>

[WebP]: <https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#webp>

[AVIF]: <https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#avif_image>

[next/image]: <https://nextjs.org/docs/api-reference/next/image>

[colocate]: <https://nextjs.org/docs/app/building-your-application/routing#colocation>

[partial rendering]: <https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#4-partial-rendering>

[client-side navigation]: <https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#how-routing-and-navigation-works>

[next/link]: <https://nextjs.org/docs/app/api-reference/components/link>

[Vercel's marketplace integrations]: <https://vercel.com/marketplace?category=databases>

[this guide on GitHub]: <https://help.github.com/en/github/getting-started-with-github/create-a-repo>

[vercel.com/signup]: <https://vercel.com/signup>

[instant preview URLs]: <https://vercel.com/docs/deployments/environments#preview-environment-pre-production#preview-urls>

[default region]: <https://vercel.com/docs/functions/configuring-functions/region>

[latency]: <https://developer.mozilla.org/en-US/docs/Web/Performance/Understanding_latency>

[bcryptjs]: <https://www.npmjs.com/package/bcryptjs>

[executing queries section]: <https://nextjs.org/learn/dashboard-app/setting-up-your-database#executing-queries>

[Route Handlers]: <https://nextjs.org/docs/app/building-your-application/routing/route-handlers>

[relational databases]: <https://aws.amazon.com/relational-database/>

[ORM]: <https://vercel.com/docs/storage/vercel-postgres/using-an-orm>

[postgres.js]: <https://github.com/porsager/postgres>

[SQL injections]: <https://github.com/porsager/postgres?tab=readme-ov-file#query-parameters>

[function]: <https://github.com/porsager/postgres>

[Promise.all()]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all>

[Promise.allSettled()]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled>

[revalidating data]: <https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#revalidating-data>

[Route Groups]: <https://nextjs.org/docs/app/building-your-application/routing/route-groups>

[dynamic function]: <https://nextjs.org/docs/app/building-your-application/routing/route-handlers#dynamic-functions>

[Suspense]: <https://react.dev/reference/react/Suspense>

[ppr]: <https://rc.nextjs.org/docs/app/api-reference/next-config-js/ppr>

[multiple methods]: <https://nextjs.org/docs/app/api-reference/functions/use-router#userouter>

[URLSearchParams]: <https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams>

[use-debounce]:  <https://www.npmjs.com/package/use-debounce>

[FormData]: <https://developer.mozilla.org/en-US/docs/Web/API/FormData>

[caching]: <https://nextjs.org/docs/app/building-your-application/caching>

[use server]: <https://react.dev/reference/react/use-server>

[couple of methods]: <https://developer.mozilla.org/en-US/docs/Web/API/FormData>

[.get(name)]: <https://developer.mozilla.org/en-US/docs/Web/API/FormData/get>

[entries()]: <https://developer.mozilla.org/en-US/docs/Web/API/FormData/entries>

[Object.fromEntries()]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries>

[Zod]: <https://zod.dev/>

[prefetching]: <https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#1-prefetching>

[revalidatePath]: <https://nextjs.org/docs/app/api-reference/functions/revalidatePath>

[redirect]: <https://nextjs.org/docs/app/api-reference/functions/redirect>

[Dynamic Route Segments]: <https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes>

[security with Server Actions]: <https://nextjs.org/blog/security-nextjs-server-components-actions>

[error.tsx]: <https://nextjs.org/docs/app/api-reference/file-conventions/error>

[Error]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error>

[Error Handling]: <https://nextjs.org/docs/app/building-your-application/routing/error-handling>

[error.js API Reference]: <https://nextjs.org/docs/app/api-reference/file-conventions/error>

[notFound() API Reference]: <https://nextjs.org/docs/app/api-reference/functions/not-found>

[not-found.js API Reference]: <https://nextjs.org/docs/app/api-reference/file-conventions/not-found>

[Learn Accessibility]: <https://web.dev/learn/accessibility/>

[web.dev]: <https://web.dev/>

[eslint-plugin-jsx-a11y]: <https://www.npmjs.com/package/eslint-plugin-jsx-a11y>

[here]: <https://nextjs.org/learn/dashboard-app/improving-accessibility#server-side-validation>

[NextAuth.js]: <https://nextjs.authjs.dev/>

[See how to setting up NextAuth.js]: <https://nextjs.org/learn/dashboard-app/adding-authentication#setting-up-nextauthjs>

[See how to setting up Next.js Middleware]: <https://nextjs.org/learn/dashboard-app/adding-authentication#protecting-your-routes-with-nextjs-middleware>

[See how to hashing a password]: <https://nextjs.org/learn/dashboard-app/adding-authentication#password-hashing>

[See how to add the Credentials provider]: <https://nextjs.org/learn/dashboard-app/adding-authentication#adding-the-credentials-provider>

[See how to add the sign in funcionality]: <https://nextjs.org/learn/dashboard-app/adding-authentication#adding-the-sign-in-functionality>

[See how to update the login form]: <https://nextjs.org/learn/dashboard-app/adding-authentication#updating-the-login-form>

[See how to add the logout functionality]: <https://nextjs.org/learn/dashboard-app/adding-authentication#adding-the-logout-functionality>

[static metadata object]: <https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadata-object>

[generateMetadata function]: <https://nextjs.org/docs/app/api-reference/functions/generate-metadata#generatemetadata-function>

[ImageResponse]: <https://nextjs.org/docs/app/api-reference/functions/image-response>

[generate-metadata documentation]: <https://nextjs.org/docs/app/api-reference/functions/generate-metadata>

[Next.js Documentation]: <https://nextjs.org/docs>

[Next.js Templates]: <https://vercel.com/templates/next.js>

[Admin Dashboard Template]: <https://vercel.com/templates/next.js/admin-dashboard-tailwind-postgres-react-nextjs>

[Next.js Commerce]: <https://vercel.com/templates/next.js/nextjs-commerce>

[Blog Starter Kit]: <https://vercel.com/templates/next.js/blog-starter-kit>

[AI Chatbot]: <https://vercel.com/templates/next.js/nextjs-ai-chatbot>

[Image Gallery Starter]: <https://vercel.com/templates/next.js/image-gallery-starter>

[Next.js Repository]: <https://github.com/vercel/next.js>

[Vercel YouTube]: <https://www.youtube.com/@VercelHQ/videos>

[Vercel Reddit]: <https://www.reddit.com/r/vercel/>