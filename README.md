# 🎯 Bliz.cc Widget Integration Guide — For Clients

Welcome! This guide explains **how Bliz.cc widgets (like spinning wheels, scratch cards, etc.) communicate with your website** — so you can capture leads, trigger actions, or redirect users based on their interaction.

No coding expertise needed — we’ll walk you through everything step by step.

---

## 🧩 How It Works — Simple Overview

1. **You embed** a Bliz widget (e.g., spinning wheel) on your site using our GTM snippet.
2. **User interacts** — spins the wheel, enters email, submits.
3. **Widget sends real-time events** to your website with structured data (promo code, user info, etc.).
4. **Your site listens** and can:
   - ✅ Close the widget automatically
   - ✅ Redirect user to checkout with promo code
   - ✅ Send lead to your CRM
   - ✅ Fire analytics or retargeting pixels
   - ✅ Trigger email/SMS sequences

---

## 📡 Events Sent to Your Website

Bliz widgets send **3 key events** during user interaction:

| Event | When It Fires | What You Can Do |
|-------|---------------|-----------------|
| `WIDGET_VIEW` | When widget loads | Track impressions, fire retargeting pixel |
| `WIDGET_INTERACTION` | When user starts playing (e.g., clicks “Spin”) | Track engagement, warm up audience |
| `WIDGET_SUBMIT` | When user completes action (e.g., submits email + wins prize) | **CLOSE WIDGET**, apply discount, send to CRM, trigger thank-you page |

> ✅ **Auto-close on Submit** — Most clients automatically close the widget overlay when `WIDGET_SUBMIT` is received.

---

## 📦 Event Payload Structure (What You Receive)

When an event is sent, your site receives a **structured JavaScript object** like this:

### TypeScript Schema (for developers)

```ts
export type WidgetEventCategory =
  | 'WIDGET_VIEW'
  | 'WIDGET_INTERACTION'
  | 'WIDGET_SUBMIT';

export interface IframePromoArgs {
  id?: string;           // Promo ID from Bliz dashboard
  title?: string;        // e.g., "Summer Sale"
  prize?: string;        // e.g., "SAVE20" (the actual promo code)
  description?: string;  // e.g., "20% off your next order"
  newPrice?: number;     // e.g., 79.99
  originalPrice?: number; // e.g., 99.99
  currency?: string;     // e.g., "USD"
  selectionMethod?: string; // e.g., "random", "weighted"
  imageUrl?: string;     // URL to prize image
  category?: string;     // e.g., "discount", "free_shipping"
}

export interface IframePayloadArgs {
  customer?: string;     // User’s email or phone (if collected)
  event: WidgetEventCategory; // One of the 3 events above
  promoId?: string;      // Index or ID of selected promo
  timestamp: string;     // ISO timestamp (e.g., "2025-02-27T10:00:00Z")
  roundId?: string;      // Unique session ID (for analytics)
  widgetId: string;      // Your widget’s unique ID (e.g., "widget_123")
  termsAccepted?: boolean; // Did user accept terms? (if applicable)
  promo?: IframePromoArgs; // Full prize/promo details (see above)
}
```


