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
