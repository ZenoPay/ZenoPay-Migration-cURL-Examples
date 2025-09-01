# 🔄 ZenoPay API Migration Guide

**Moving from Old API (`api.zeno.africa`) to New API (`zenoapi.com`)**

This guide shows exactly **what to change** in your integration with side-by-side examples.

---

## 1. Creating an Order & Initiating Payment

### Old API (before)

```bash
curl -X POST https://api.zeno.africa \
  -d "create_order=1" \
  -d "buyer_email=iam@gmail.com" \
  -d "buyer_name=John Doe" \
  -d "buyer_phone=0742920510" \
  -d "amount=1000" \
  -d "account_id=YOUR_ACCOUNT_ID" \
  -d "api_key=YOUR_API_KEY" \
  -d "secret_key=YOUR_SECRET_KEY"
```

### New API (now)

```bash
curl -X POST "https://zenoapi.com/api/payments/mobile_money_tanzania" \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "order_id": "3rer407fe-3ee8-4525-456f-ccb95de38250",
    "buyer_email": "iam@gmail.com",
    "buyer_name": "John Joh",
    "buyer_phone": "0744963858",
    "amount": 1000,
    "webhook_url": "https://your-domain.com/payment-webhook"
  }'
```

### ✅ What Changed

* **URL**: `https://api.zeno.africa` → `https://zenoapi.com/api/payments/mobile_money_tanzania`
* **Auth**: `api_key` + `secret_key` in body → **x-api-key in header**
* **Request type**: `form-data` → **JSON body**
* **order\_id**: You must generate a **UUID** for each transaction
* **Webhook**: You can now pass `webhook_url` directly in the request

---

## 2. Checking Order Status

### Old API (before)

```bash
curl -X POST https://api.zeno.africa/order-status \
  -d "check_status=1" \
  -d "order_id=66c4bb9c9abb1" \
  -d "api_key=YOUR_API_KEY" \
  -d "secret_key=YOUR_SECRET_KEY"
```

### New API (now)

```bash
curl -X GET "https://zenoapi.com/api/payments/order-status?order_id=3rer407fe-3ee8-4525-456f-ccb95de38250" \
  -H "x-api-key: YOUR_API_KEY"
```

### ✅ What Changed

* **Method**: `POST` → **GET**
* **Auth**: No more `secret_key` or body params → just `x-api-key` header
* **Params**: Send `order_id` as **query string**, not in body
* **Response**: Now includes richer details like `resultcode`, `result`, `data[]`

---

## 3. Webhook Handling

### Old Webhook Payload

```json
{
  "order_id": "66c4bb9c9abb1",
  "payment_status": "PENDING"
}
```

### New Webhook Payload

```json
{
  "order_id": "677e43274d7cb",
  "payment_status": "COMPLETED",
  "reference": "1003020496",
  "metadata": {
    "product": "YourProductName",
    "amount": "1000"
  }
}
```

### ✅ What Changed

* **Security**: Webhooks now include `x-api-key` in headers — verify this on your server.
* **Status**: Webhooks only fire on **COMPLETED** payments (not every status).
* **Extra data**: You now get `reference`, `channel`, `metadata`, etc.

---

## 🔍 Migration Checklist

1. **Replace Base URLs**

   * `https://api.zeno.africa` → `https://zenoapi.com/api/payments/...`

2. **Switch Authentication**

   * Remove `account_id` + `secret_key`
   * Use **x-api-key header**

3. **Update Payloads**

   * Use **JSON instead of form-data**
   * Generate a **UUID order\_id**

4. **Update Order Status Calls**

   * Change `POST /order-status` → **GET /order-status?order\_id=...**

5. **Update Webhook**

   * Verify `x-api-key`
   * Expect payload only when `payment_status=COMPLETED`

---
