# Setting up the matching Automatic Discounts

The Bundle & Save block only shows a **preview** of the discounted price in the UI. The real
discount that gets applied at checkout comes from Shopify's built-in **Automatic Discounts**
engine, configured in the Admin — no app, no code.

This needs to be set up **once per product** (or per collection/tag, if you extend the block to
target more than one product).

## Step-by-step

For each bundle tier (e.g. "2 pairs → 20% off", "3 pairs → 25% off"):

1. Go to **Shopify Admin → Discounts → Create discount → Amount off products**.
2. **Method**: choose **Automatic discount** (not "Discount code" — customers shouldn't need to
   type anything).
3. **Title**: something customer-facing and clear, e.g. `2 pairs Save 20%`. This text is shown to
   the customer in their cart and at checkout.
4. **Discount value**: `Percentage`, set to match the tier (e.g. `20`).
5. **Applies to**: `Specific products` → search for and select the product(s) this bundle applies to.
6. **Minimum purchase requirements**: ⚠️ this is the step that's easy to miss.
   - Select **`Minimum quantity of items`** (NOT "No minimum requirements").
   - Enter the tier's quantity threshold (`2` for the 20% tier, `3` for the 25% tier).

   If this is left on "No minimum requirements", the discount will incorrectly apply even to a
   single-item purchase.
7. **Combinations**: leave at the default (discount won't combine with other product/order/shipping
   discounts). This is what makes tier-stacking behave correctly — see below.
8. Save.

Repeat for every tier your bundle block offers.

## Why higher tiers "win" automatically

Because each discount requires a *minimum* quantity (not an *exact* quantity) and discounts in
this setup don't combine with each other, Shopify's engine will always apply the **highest-value
discount the cart currently qualifies for**:

| Quantity in cart | Discounts that qualify | Discount applied |
|---|---|---|
| 1 | none | — |
| 2 | "2 pairs Save 20%" | 20% |
| 3 | "2 pairs Save 20%" **and** "3 pairs Save 25%" | 25% (highest of the two) |

No extra configuration is needed to make this happen — it's default Shopify discount-engine
behavior as long as "Minimum quantity of items" is used consistently across all tiers.

## Verifying it worked

1. On the storefront, select the "2 pairs" (or higher) bundle option and click Add to Cart.
2. Go to the cart or checkout page.
3. You should see a line item discount matching the tier's title (e.g. "2 pairs Save 20%") and the
   total should reflect the discounted amount — not just quantity × full price.
