# SupportDesk

A fast, accessible customer-support portal built with **React 18, TypeScript and Redux Toolkit**. Users can search support articles, filter by product, and book an in-store visit. It is a portfolio project that mirrors the kind of work a customer-support web team does: search at scale, forms people can actually complete, and UI that works for everyone.

> Demo project with fictional content. It is not affiliated with, or endorsed by, any company.

## Features

- **Instant article search** with a 200 ms debounce, product filters, and a live result count.
- **Booking form** with inline validation, focus moved to the first error, and a confirmation screen.
- **Loading, empty and error states** (skeleton placeholders, retry button) so the UI never feels broken.
- **Light, dark and system themes**, remembered between visits.
- **Fully keyboard-accessible** with a skip link, visible focus rings, and reduced-motion support.

## Tech stack

| Area | Choice |
| --- | --- |
| UI | React 18, TypeScript (strict mode) |
| State | Redux Toolkit (`createSlice`, `createAsyncThunk`, memoised selectors) |
| Build | Vite 5 |
| Tests | Vitest + React Testing Library + user-event (Jest-compatible API) |
| Quality | ESLint (with `jsx-a11y` and React Hooks rules), `tsc --noEmit` |
| CI/CD | GitHub Actions (lint, typecheck, test, build) |

## Getting started

Requires Node.js 18 or newer.

```bash
npm install
npm run dev        # http://localhost:5173
```

| Script | What it does |
| --- | --- |
| `npm run dev` | Start the dev server |
| `npm run build` | Typecheck, then create a production build in `dist/` |
| `npm run preview` | Serve the production build locally |
| `npm test` | Run all tests once |
| `npm run test:watch` | Re-run tests on change |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | Run the TypeScript compiler without emitting files |

## Project structure

```
src/
  api/          Typed fetch helper and the booking API stub
  components/   Feature components (ArticleSearch, ArticleCard, AppointmentForm)
    ui/         Reusable building blocks (Button, TextField, SelectField, TextAreaField)
  hooks/        useDebounce, useTheme
  lib/          Pure functions (form validation)
  store/        Redux Toolkit slices, typed hooks, store factory
  test/         Unit and component tests
public/api/     Static JSON that stands in for a REST backend
```

## Design decisions

- **Redux Toolkit for server data.** Articles are fetched once through `createAsyncThunk`. A `condition` check stops duplicate requests (React StrictMode runs effects twice in development).
- **Memoised selector.** `selectVisibleArticles` only recomputes when items, query or category change.
- **Pure validation.** `validateAppointment` has no UI dependencies, so it is trivial to test and could be reused on a server.
- **Reusable form fields.** One shared shell wires labels, `aria-invalid` and `aria-describedby`, so every field is accessible by default.
- **Code splitting.** The booking form loads with `React.lazy`, so the first page load only downloads what it needs.
- **`makeStore()` factory.** Every test gets a fresh store, so tests never leak state into each other.
- **Native `<details>`** for expandable articles: keyboard and screen-reader support with no custom JavaScript.

## Accessibility

Visible labels on every field, errors announced and linked to their inputs, status messages in live regions, 3 px focus outlines, colour tokens with sufficient contrast in both themes, and `prefers-reduced-motion` respected. `eslint-plugin-jsx-a11y` runs in CI to catch regressions. Automated checks cannot find everything, so please also test with a screen reader.

## Testing

Tests cover the Redux slice and selector, the validation rules, search and filter behaviour, error and retry handling, and the booking flow including focus management.

```bash
npm test
```

## Connecting a real backend

The demo reads articles from `public/api/articles.json` and simulates `POST /appointments` in `src/api/client.ts`. To go live, point `getJSON` at your API and replace the body of `bookAppointment` with a real `fetch` call. The rest of the app does not change.

## Roadmap

- End-to-end tests with Playwright
- Pagination or virtualised list for very large article sets
- Web Vitals reporting (LCP, INP, CLS)
- Storybook for the UI components
- Internationalisation

## License

MIT
