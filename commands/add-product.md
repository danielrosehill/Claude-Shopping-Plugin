Read the user's catalog screenshot(s) from the path they provide as $ARGUMENTS.

Extract every visible product: name, brand, price (regular + sale/Eilat if shown), store name. Rename the screenshot to a descriptive filename based on store and products shown.

Then update `products.json` (create if missing) by appending the new products. Avoid duplicates — match on brand + model + store. Output a summary of what was added.