## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

## Learnings

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
- Adding images manually means the following needs to be optimized for manually
  - Image responsivity on different screens
  - Specification of different sizes for different screens
  - Prevent layout shift as images load
  - Lazy loading of images outside users viewport
- The `<Image>` Component from Next does all of this optimization by default
  - Images are lazy loaded by default
  - They are made responsive they scale based on screen
  - Images served in latest formats like `WebP` and `AVIF` when supported by browsers
  - Prevents layout shift automatically when loading images
    - For this, its good practice to set image width and height in a aspect ratio identical to that of the source image
