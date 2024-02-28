## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

## Notes

1. CSS Styling

- Global styles can be added at the root layout level
- NextJS uses Tailwind and CSS modules as defaults for styling of the app
  - Tailwind is a framework to add utility classes directly in TSX
  - CSS Modules allows for CSS to be scoped to a component, generating unique classnames, reducing clash in classnames from different modules
  - The above two can also be combined in a given NextJS app
  - Other frameworks, as well as CSS-in-JS libraries can also be used for styling of the app
  - Use the library `clsx` to help apply styles conditionally in TSX

2. Optimizing Fonts and Images

- Custom fonts are downloaded and stored as static assets during app build by NextJS
- This allows for the font to be readily available to the app everytime it is loaded, reducing at least one network call
- This also causes there to be no Cumulative Layout Shift (Layout shift created by the browser initially loading the app with a fallback font, then replacing it with the custom font once the network call has been completed)
- Add google fonts based on their properties by importing them and applying them at the level required
- Multiple fonts can be applied throughout the app. Ex: One font on the top level, that can be overridden by another font that is added to one or two specific elements deep in the apps dom.
- Image Optimization in web apps is a specialization in itself
- Static assets like images are usually stored in the public folder
- Adding images manually means the following needs to be optimized for manually
  - Image responsivity on different screens
  - Specification of different sizes for different screens
  - Prevent layout shift as images load
  - Lazy loading of images outside users viewport
- The `<Image>` Component from Next does all of this optimization by default
  - Images are lazy loaded by default
  - They are made responsive, they scale based on screen
  - Images served in latest formats like `WebP` and `AVIF` when supported by browsers
  - Prevents layout shift automatically when loading images
    - For this, its good practice to set image width and height in a aspect ratio identical to that of the source image

3. Layouts and Pages

- Next uses file-system routing, where folders are used to create nested routes
- Each folder is a route segment that is part of the URL segment
- Create separate UIs for each route with a `page.tsx` and `layout.tsx` file
  - the path `/` corresponds to `app/page.tsx`, the path `/dashboard` corresponds to `app/dashboard/page.tsx`
  - creating nested routes is done by creating folders with appropriate names and a `page.tsx` (if the segment is to be routable)
- You can colocate logically relevant files and folders without making them routable simply by not adding a `page.tsx`
- `layout.tsx` is used to specify UI that is shared by multiple pages
- The children to a layout can themselves either be pages or layouts
- You define the part of the UI that does not change for a particular route in its respective `layout.tsx`

  - This causes only parts of the page to update while the layout itself is not re-rendered
  - This effect is called partial rendering

4. Navigating between Pages

- Using a `<a>` to link and navigate between pages causes a full page refresh because the code is mostly server-side rendered
- `<Link />` from NextJS allows for client-side navigation in JS
- This does not allow a page to undergo full refresh even if some components in it are server-side rendered
  - Because of automatic code-splitting by route segments, pages become isolated
  - If a page throws an error, the rest of the app will still work
  - Because the app is mostly server-side rendered, the app will work reasonably even if theres a problem with one page
  - In production, when there is a `<Link>` in the browsers viewport, NextJS prefetches the code for the linked route - making the code readily available for the user in less than a few milliseconds!
- UI Pattern: Show active link to indicate to user what page they are currently on
  - NextJS provides the `usePathname` hook for this
  - To use hooks, the component has to be converted from a server-side rendered to a client-side rendered one
  - use `use client;` at the top of the component definition to indicate this to NextJS

5. Setting up DB, deploying the app in Vercel

- Connect your github, select a project to deploy, name the project and directly deploy (for a minimal deployment)
- Everytime a branch is created, a preview of the app with the changes on the branch can be viewed in Vercel to catch possible errors before actual deployment
- The default region in which the app is deployed is Washington D.C, so other services connected to the app should ideally be deployed close to this region for low-latency
- Once the db is created the region cannot be reset - so be careful!
- Copy the secrets generated and available in the dashboard to the appropriate .env files, keep the .env files gitignored
- Seeding the db is adding some initial data to it

6. Fetching Data

- When fetching data on the client, do not query the database directly as you risk exposing DB secrets this way
- You can interact with the DB via an API layer, which would require you to write queries
- While interacting with relational DBs can be done with SQL or ORMS like Prisma
- Fetching data via React Server Components is the same as fetching data onthe server, then you can skip the API layer and query the DB directly
- Server Components support promises, allowing simpler interface to async tasks like data fetching
- Easily allows fetching data using simple `async/await` to fetch data instead of needing to use `useEffect` or `useState` in combination with data fetching libraries like axios
- Using Server Components without a API layer, you can write extensive queries that do all the heavy lifting on the server side directly

  - App is much faster as you won't be doing any in-memory tasks unless it is absolutely necessary
  - Its good practice to delegate as many jobs to the appropriate workers than to let the frontend do the heavy-lifting

- Request Waterfalls: Sequence of network requests that depend on the completion of previous requests
  - There may be cases where you want this to happen, so it is not necessarily a bad pattern.
  - Intentionality is key
- Parallel Data Fetching:
  - use `Promise.all()` or `Promise.allSettled()` to make data requests at the same time
  - This works smoothly except for in cases where one or two of the requests are slower than the others.

7. Static and Dynamic Rendering

- Static Rendering is when data fetching and rendering happens on the server at build time (when app is deployed) or during revalidation (when data cache is purged and a new data is deployed resulting in distribution and caching in CDNs)
  - Cached sites implies faster sites
  - Because content is cached, server load is reduced as it does not have to generate and send content for every request for the site
  - Pre-rendered sites (statically rendered sites) are easy for search engine crawlers to index, hence making the sight SEOd
- Static rendering is useful for UI with no data or data that is shared across users

- Dynamic Rendering is when content is rendered on the server for each user at request time (when user visits the page)
  - This provides real-time data
  - Its easier to save user specific content, and update data based on user interaction
  - It allows access to information available only at request time like cookies , URL search params
- By default NextJS apps are static rendered, you have to manually opt in for dynamic rendering when required
- Use NextJS API `unstable_noStore` in Server Components or in data fetching functions to opt out of static rendering
- With dynamic rendering your page load is only as fast as your slowest data fetch!
