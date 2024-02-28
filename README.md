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
