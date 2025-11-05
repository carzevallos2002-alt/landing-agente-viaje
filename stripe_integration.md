Stripe integration (guide - test mode)

This document explains how to integrate Stripe Checkout in test mode for the Agente de Viaje demo.

1) Install dependencies

In your Python environment (project root):

```
pip install stripe flask python-dotenv
```

2) Minimal Flask server (create file `stripe_server.py`)

```python
# stripe_server.py
import os
from flask import Flask, jsonify, request
import stripe

app = Flask(__name__)
stripe.api_key = os.environ.get('STRIPE_SECRET_KEY')  # set your test secret key here

@app.route('/create-checkout-session', methods=['POST'])
def create_checkout():
    data = request.get_json() or {}
    price = int(data.get('price_usd', 199) * 100)
    session = stripe.checkout.Session.create(
        payment_method_types=['card'],
        mode='payment',
        line_items=[{
            'price_data': {
                'currency': 'usd',
                'product_data': {'name': 'Piloto AgenteViaje (4 semanas)'},
                'unit_amount': price,
            },
            'quantity': 1,
        }],
        success_url='https://example.com/success',
        cancel_url='https://example.com/cancel',
    )
    return jsonify({'url': session.url})

if __name__ == '__main__':
    app.run(port=4242)
```

3) Run locally (set test keys first)

Windows cmd:

```
set STRIPE_SECRET_KEY=sk_test_...
python stripe_server.py
```

4) In `streamlit_app.py` you can call this server endpoint to receive the session URL and open it for the user. Alternatively, set `STRIPE_SECRET_KEY` in the environment and the demo will try to create the Checkout Session directly (if `stripe` package is installed).

Security note: never commit your secret key to the repo. Use environment variables or a secret manager.

Webhooks: for production, implement and verify webhooks to record successful payments server-side and mark pilots as paid.
