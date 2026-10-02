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

Everything is stored in your browser's localStorage on your own device. Nothing is sent to a server, apart from food search and barcode lookups sent to Open Food Facts and USDA FoodData Central. Clearing site data erases it, so use **Settings → Backup** to keep a copy.

## Run it

Open `index.html` in a browser, or visit the GitHub Pages site for this repository. Camera barcode scanning needs HTTPS (which GitHub Pages provides) and a browser that supports `BarcodeDetector`, such as Chrome on Android.

## Credits

Food data from [Open Food Facts](https://world.openfoodfacts.org) (ODbL) and [USDA FoodData Central](https://fdc.nal.usda.gov).
