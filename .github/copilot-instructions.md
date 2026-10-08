# Repository guidance

## Commands

This repository uses npm (`package-lock.json`) and Expo SDK 57. Available package scripts:

- `npm start` — start the Expo development server.
- `npm run android` — start Expo targeting Android.
- `npm run ios` — start Expo targeting iOS.
- `npm run web` — start Expo targeting web.

There is currently no configured test runner, single-test command, lint script, or typecheck script. Check `package.json` before assuming any of these are available.

## App structure and data flow

- `index.js` registers `App.js` with Expo. `App` first renders the welcome screen, then mounts `NavigationContainer` and the tab navigator after the user taps “Começar”. That onboarding flag is React state only; it is not persisted across launches.
- `routes/TabRoutes.js` defines the four tabs. The Dashboard tab contains a nested native stack in `routes/DashboardStack.js`, with the dashboard list and transaction detail screens.
- The dashboard owns the current transaction list in React state and starts from sample data in `screens/DashboardScreen.js`. `screens/NovaTransacaoScreen.js` creates a transaction and sends it to the nested Dashboard screen through route params; the dashboard prepends it when the param changes. `screens/DetalheTransacaoScreen.js` receives a selected transaction through route params.
- The report currently uses separate hardcoded totals in `screens/RelatorioScreen.js`. There is no persistence or shared transaction store wired up yet; keep data behavior consistent across screens when changing this flow.
- Shared visual tokens live in `theme.js`; reusable transaction and balance summary UI lives in `components/`. Screen-specific UI and styles live with each screen.

## Repository conventions

- Use React Navigation (bottom tabs plus a native stack) for navigation, preserving the existing route names and nested route-param shape. This app does not use Expo Router.
- Screen and component modules use named exports. `App.js` is the default-exported app root.
- The interface, user-facing text, and most domain names are in Portuguese. Transaction `tipo` values are `'receita'` and `'despesa'`; categories use the IDs defined in `screens/NovaTransacaoScreen.js`.
- Use React Native components and `StyleSheet.create`; reuse the color, spacing, and radius tokens from `theme.js`. Safe-area handling is provided by `react-native-safe-area-context`, and icons use `@expo/vector-icons` Ionicons.
- Before changing Expo, EAS, or React Native APIs, read the Expo major version from `package.json` and consult the matching versioned docs at `https://docs.expo.dev/versions/v57.0.0/`. Use `npx expo install <package>` for Expo-compatible dependency versions. If native configuration is needed, prefer `app.json` or a config plugin; do not hand-create native project directories.
