# Google Analytics E-commerce Tracking Examples

## Generic Purchase Tracking Function

Generic function to track any purchase in the app:

```javascript
// Generic function to track any purchase in the app
function trackPurchase(orderData) {
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({ ecommerce: null }); // Clear the previous ecommerce object (important!)
  
  window.dataLayer.push({
    event: 'purchase',
    ecommerce: {
      transaction_id: orderData.id,
      value: orderData.totalPrice,
      currency: orderData.currencyCode,
      items: orderData.items.map(item => ({
        item_id: item.sku,
        item_name: item.name,
        price: item.price,
        quantity: item.qty
      }))
    }
  });
}
```

## React Component Example

React component that tracks purchases using `useEffect`:

```javascript
import React, { useEffect } from 'react';

const SuccessComponent = ({ transaction }) => {
  useEffect(() => {
    // Ensure dataLayer exists
    window.dataLayer = window.dataLayer || [];

    // 1. Clear previous ecommerce data
    window.dataLayer.push({ ecommerce: null });

    // 2. Push new purchase data
    window.dataLayer.push({
      event: 'purchase',
      ecommerce: {
        transaction_id: transaction.id,
        value: transaction.amount,
        currency: 'USD',
        items: transaction.products.map(p => ({
          item_id: p.id,
          item_name: p.name,
          price: p.price,
          quantity: 1
        }))
      }
    });
  }, [transaction]); // Runs once when the transaction data loads

  return <div>Thank you for your purchase!</div>;
};
```

## Purchase Event Data Structure Example

Example of the complete purchase event object structure:

```json
{
    "event": "purchase",
    "ecommerce": {
      "transaction_id": "T_12345", // Unique ID for the order
      "value": 49.00,             // Total revenue (Number, not string)
      "tax": 4.90,                // Optional
      "shipping": 0.00,           // Optional
      "currency": "USD",          // 3-letter ISO 4217 format
      "items": [
        {
          "item_id": "SKU_12345",   // Internal product ID
          "item_name": "Pro Plan", // Name of the SaaS plan/add-on
          "item_category": "SaaS", // General category
          "price": 49.00,          // Price of this specific item
          "quantity": 1            // Usually 1 for SaaS
        }
      ]
    }
  }
```

## Notes

- Always clear the previous ecommerce object by pushing `{ ecommerce: null }` before pushing new purchase data
- The `dataLayer` must be initialized before use: `window.dataLayer = window.dataLayer || []`
- In React components, use `useEffect` to ensure tracking happens after the component mounts and transaction data is available
- `value` should be a Number, not a string
- `currency` should be in 3-letter ISO 4217 format (e.g., "USD", "EUR")
- `tax` and `shipping` are optional fields
- For SaaS products, `quantity` is usually 1

