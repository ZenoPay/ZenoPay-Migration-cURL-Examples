# 🔄 ZenoPay Migration – cURL Examples

## 1. Creating an Order & Initiating Payment

### Old API

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

### New API

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

---

## 2. Checking Order Status

### Old API

```bash
curl -X POST https://api.zeno.africa/order-status \
  -d "check_status=1" \
  -d "order_id=66c4bb9c9abb1" \
  -d "api_key=YOUR_API_KEY" \
  -d "secret_key=YOUR_SECRET_KEY"
```

### New API

```bash
curl -X GET "https://zenoapi.com/api/payments/order-status?order_id=3rer407fe-3ee8-4525-456f-ccb95de38250" \
  -H "x-api-key: YOUR_API_KEY"
```

---

## 3. Webhook Payload Example

### Old API Webhook Sample

```json
{
  "order_id": "66c4bb9c9abb1",
  "payment_status": "PENDING"
}
```

### New API Webhook Sample

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
