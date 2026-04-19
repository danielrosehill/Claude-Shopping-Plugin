# Israeli Tech Shopping Rules

## VAT (מע"מ)

Israeli VAT is **18%** (as of 2025).

### Detecting ex-VAT prices
Some retailers show prices excluding VAT. Look for these Hebrew/English indicators:
- לפני מע"מ / לפני מע״מ (before VAT)
- בתוספת מע"מ (plus VAT)  
- ללא מע"מ (without VAT)
- לא כולל מע"מ (not including VAT)
- + מע"מ / +מע״מ
- ex VAT / excl. VAT / before VAT / + VAT

If any of these appear near a price, multiply by 1.18 to get the real price.

### Eilat pricing
KSP and some other retailers show a lower "Eilat price" (מחיר אילת) which excludes VAT — this price is only valid for Eilat residents. **Always use the higher (regular) price** that applies to the rest of Israel.

When you see two prices on KSP, the higher one is the regular price.

## Currency

- Israeli prices are in ILS (₪, שקלים)
- For USD/ILS conversion, use the exchange rate API: `https://api.exchangerate-api.com/v4/latest/USD` — check `rates.ILS`
- Fallback rate if API is down: ~3.65 ILS per USD

## Price comparison methodology

1. Always compare prices **including VAT**
2. Sort results by price (inc. VAT) ascending
3. When comparing to international RRP:
   - ≤5% markup → GOOD DEAL (at or near international price)
   - 5-15% → FAIR (typical Israel markup for imported tech)
   - 15-30% → ABOVE AVERAGE (noticeable markup)
   - >30% → EXPENSIVE (significant markup, consider buying internationally)

## Translation

Israeli retailer search works best with **Hebrew queries**. You (Claude) speak Hebrew natively — translate the user's English product query to Hebrew before searching. Common patterns:
- Use the Hebrew product category name
- Keep brand names and model numbers in English/Latin script (e.g., "סוללות נטענות Eneloop AA")
- Israeli sites understand mixed Hebrew+English queries

## Branch stock checking

Some retailers show branch/store availability. When a user specifies their city:
- Look for branch availability info on product pages
- Common Hebrew terms: סניף (branch), זמינות (availability), איסוף (pickup), במלאי (in stock)
- Match city names in both Hebrew and English

### Common city mappings
| English | Hebrew |
|---------|--------|
| Jerusalem | ירושלים |
| Tel Aviv | תל אביב |
| Haifa | חיפה |
| Beer Sheva | באר שבע |
| Rishon LeZion | ראשון לציון |
| Petah Tikva | פתח תקווה |
| Netanya | נתניה |
| Ashdod | אשדוד |
| Bnei Brak | בני ברק |
| Holon | חולון |
| Ramat Gan | רמת גן |
| Rehovot | רחובות |
| Ashkelon | אשקלון |
| Herzliya | הרצליה |
| Kfar Saba | כפר סבא |
| Ra'anana | רעננה |
| Modi'in | מודיעין |
| Eilat | אילת |
