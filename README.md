# Amazon Product Details Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape Amazon.com product pages from ASINs or links: title, brand, price, list price, rating, ratings count, stock status, features, description, images, categories, best sellers rank, variations and product details.

This repository has ready scripts for the [Amazon Product Details Scraper: ASIN, Price, Sales Rank](https://apify.com/v0iddo/amazon-product-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/amazon-product-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "products": [
    "B09B8V1LZ3",
    "0735211299",
    "B00FLYWNYQ",
    "B09HM94VDS",
    "B0CFPJYX7P",
    "B0BP9SNVH9"
  ],
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `products` | array | Amazon.com ASINs such as B09B8V1LZ3, or product links such as https://www.amazon.com/dp/B09B8V1LZ3 (any link with /dp/ or /gp/product/ and the ASIN in it). |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "asin": "B09B8V1LZ3",
  "url": "https://www.amazon.com/dp/B09B8V1LZ3",
  "title": "Amazon Echo Dot - Vibrant sounding speaker, Designed for Alexa+, Great for bedrooms, dining rooms and offices, Charcoal",
  "brand": "Amazon",
  "price": 79.99,
  "currency": "USD",
  "listPrice": null,
  "rating": 4.7,
  "ratingsCount": 201590,
  "availability": "In Stock",
  "inStock": true,
  "features": [
    "Your favorite music and content – Play music, audiobooks, and podcasts from Amazon Music, Apple Music, Spotify and others or via Bluetooth throughout your home.",
    "Alexa is happy to help – Ask Alexa for weather updates and to set hands-free timers, get answers to your questions and even hear jokes. Need a few extra minutes in the morning? Just tap your Echo Dot to snooze your alarm.",
    "Keep your home comfortable – Control compatible smart home devices with your voice and routines triggered by built-in motion or indoor temperature sensors.",
    "Do more with device pairing – Fill your home with music using compatible Echo devices in different rooms, or create a home theatre system with Fire TV.",
    "Say goodbye to drop-offs and buffering - With eero Built-in, Echo Dot doubles as a mesh wifi extender, adding up to 1,000 sq. ft. of wifi coverage to your existing eero network."
  ],
  "description": "Enhance your smart home hub experience with the Charcoal colored 2022 Echo Dot 5th generation smart speaker featuring Alexa capabilities from Amazon. Shop today.",
  "images": [
    "https://m.media-amazon.com/images/I/61J2sQtBYDL._AC_SL1500_.jpg",
    "https://m.media-amazon.com/images/I/71hNp8d9WvL._AC_SL1500_.jpg",
    "https://m.media-amazon.com/images/I/61qr4W1Hb0L._AC_SL1500_.jpg",
    "https://m.media-amazon.com/images/I/716Ncf4NDFL._AC_SL1500_.jpg",
    "https://m.media-amazon.com/images/I/61D9R4b2FyL._AC_SL1500_.jpg"
  ],
  "mainImage": "https://m.media-amazon.com/images/I/61J2sQtBYDL._AC_SL1500_.jpg",
  "categories": [],
  "bestSellersRank": [],
  "variationsCount": 6,
  "variationAsins": [
    "B09B8V1LZ3",
    "B0HKL4CT1J",
    "B09B94RL1R",
    "B0HKTHK7D6",
    "B0HKLPHJ2L"
  ],
  "dimensions": null,
  "weight": null,
  "model": null,
  "manufacturer": null,
  "dateFirstAvailable": null,
  "specifications": {}
}
```

Fields: `asin`, `url`, `title`, `brand`, `price`, `currency`, `listPrice`, `rating`, `ratingsCount`, `availability`, `inStock`, `features`, `description`, `images`, `mainImage`, `categories`, `bestSellersRank`, `variationsCount`, `variationAsins`, `dimensions`, `weight`, `model`, `manufacturer`, `dateFirstAvailable`, `specifications`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/amazon-product-scraper').call({"products": ["B09B8V1LZ3", "0735211299", "B00FLYWNYQ", "B09HM94VDS", "B0CFPJYX7P", "B0BP9SNVH9"], "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~amazon-product-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"products": ["B09B8V1LZ3", "0735211299", "B00FLYWNYQ", "B09HM94VDS", "B0CFPJYX7P", "B0BP9SNVH9"], "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$1.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
