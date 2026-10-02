# Macro Tracker

A private, no-account calorie, protein, carb and fat tracker. It is a single `index.html` file with no build step and no dependencies.

## Features

- Daily calorie and protein goals (plus carbs and fat), with rest and training day goals
- Fast logging: repeat yesterday, saved foods and meals, quick add, swipe to delete with undo
- Food search (Open Food Facts, with a USDA FoodData Central fallback) and barcode scanning
- Optional fibre, sugar and sodium tracking
- Weight trend, weekly insights, streaks and a consistency heatmap
- Goal calculator, kg/lb units, light and dark mode
- JSON backup/import and CSV export

## Your data

By default everything is stored in your browser's localStorage on your own device. Optional **GitHub sync** (Settings → History sync) also keeps your history in a `tracker-data.json` file on this repository's `data` branch, so it survives clearing the browser and follows you across devices. You add a fine-grained GitHub token (this repo only, Contents read/write) in Settings. It is stored only on that device and is never written to the repo or to backups. Because this repository is public, the synced history is publicly readable.

Apart from optional sync, nothing is sent anywhere except food search and barcode lookups to Open Food Facts and USDA FoodData Central. Clearing site data erases the local copy, so keep sync on or use **Settings → Backup**.

## Run it

Open `index.html` in a browser, or visit the GitHub Pages site for this repository. Camera barcode scanning needs HTTPS (which GitHub Pages provides) and a browser that supports `BarcodeDetector`, such as Chrome on Android.

## Credits

Food data from [Open Food Facts](https://world.openfoodfacts.org) (ODbL) and [USDA FoodData Central](https://fdc.nal.usda.gov).
