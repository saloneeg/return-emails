# Return Email Mapping Guide

This document maps return states/events to email templates for engineering implementation.

## Email Templates Overview

| Email Template | Trigger Event | HTML File |
|---|---|---|
| Return Confirmation - DX Pickup | Return request confirmed, Dasher pickup scheduled | `return-confirmation-dx-pickup.html` |
| Return Confirmation - In-Store | Return request confirmed, in-store drop-off required | `return-confirmation-in-store.html` |
| Return Complete - DX Pickup | Dasher picked up items, return complete | `return-complete-dx-pickup.html` |
| Sephora Has Your Return | Merchant received items (in-store drop-off), processing refund | `return-complete-in-store.html` |
| Your Return Is Complete | Refund approved and issued | `refund-on-the-way-in-store.html` |

## Detailed Email Flow

### DX Pickup Flow
1. **Customer initiates return** → Send `return-confirmation-dx-pickup.html`
   - Subject: "Sephora Return Request Confirmation"
   - Heading: "We got your return request"
   - CTA: "Track your return"

2. **Dasher picks up items** → Send `return-complete-dx-pickup.html`
   - Subject: "Sephora return complete"
   - Heading: "Your return is complete"
   - CTA: "View return details"
   - Body: Refund on the way (4-7 business days)

### In-Store Drop-off Flow
1. **Customer initiates return** → Send `return-confirmation-in-store.html`
   - Subject: "Sephora in-store return request"
   - Heading: "We got your return request"
   - CTA: "Track your return"
   - Body: Includes drop-off deadline and return policy link

2. **Merchant receives items** → Send `return-complete-in-store.html`
   - Subject: "Sephora has your return"
   - Heading: "Sephora has your return"
   - CTA: "Track your return"
   - Body: Processing return, will send update when refund is ready

3. **Refund approved** → Send `refund-on-the-way-in-store.html`
   - Subject: "Sephora return complete"
   - Heading: "Your return is complete"
   - CTA: "View return details"
   - Body: Refund on the way (4-7 business days)

## Dynamic Content Variables

All emails require these variables to be populated:

```javascript
{
  customerName: "Salonee",           // Customer first name
  merchantName: "Sephora",           // Merchant name
  refundAmount: "$34.99",            // Total refund amount
  paymentMethod: "Visa ****1234",    // Payment method last 4 digits
  items: [                           // Array of returned items
    {
      name: "Product Name",
      variant: "Color/Size details",
      quantity: 1,
      price: "$34.99",
      imageUrl: "https://..."
    }
  ],
  dropoffDeadline: "3/12/2026",     // Only for in-store confirmation
  returnPolicyUrl: "https://www.sephora.com/returns"  // Merchant return policy
}
```

## Button Links

- **Track your return**: `https://www.doordash.com/returns/track`
- **View return details**: `https://www.doordash.com/orders/return-details`
- **Get Return Help**: `https://www.doordash.com/help`

## Notes for Implementation

1. **Return policy links**: The confirmation emails include merchant-specific return policy links. Make sure to populate this with the correct merchant URL.

2. **Refund timing**: All emails with refund information state "4-7 business days" - this should be configurable if merchants have different refund timelines.

3. **Email subject lines**: All include merchant name for clarity (e.g., "Sephora return complete").

4. **Button text**: Uses sentence case throughout ("Track your return" not "Track Your Return").

5. **Dark mode**: All templates include dark mode support via media queries.

## Testing Checklist

- [ ] Customer name populates correctly
- [ ] Merchant name appears in subject line and body
- [ ] Refund amount displays with correct currency
- [ ] Payment method shows last 4 digits
- [ ] Item images load correctly
- [ ] All CTAs link to correct URLs
- [ ] Drop-off deadline displays for in-store returns
- [ ] Return policy link works
- [ ] Dark mode renders correctly
- [ ] Mobile responsive layout works

## Live Templates

View all templates at: https://saloneeg.github.io/return-emails/
