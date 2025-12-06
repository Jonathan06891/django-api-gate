Django API Gate 🏦

Monetize your Django API in minutes.

django-api-gate is a drop-in middleware that turns your API into a metered, paid service.

It connects to the API Gate Hub protocol to handle API keys, credit balances, and billing automatically.

🚀 Features

Instant Paywall: Protects your API routes with one line of code.

Metered Billing: Deducts credits per request automatically.

Zero-Config Banking: No need to set up Stripe or Ledger logic yourself—API Gate Hub handles the backend.

High Performance: Uses persistent connection pooling for minimal latency impact (~20ms).

📦 Installation

pip install django-api-gate


⚙️ Configuration

1. Add Middleware

In your settings.py:

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # ...
    'django_api_gate.middleware.ApiGateMiddleware', # <--- Add this
    # ...
]


2. Add Your Merchant Key

Go to API Gate Hub and create an account.

Copy your API Key from the dashboard.

Add it to settings.py:

API_GATE_API_KEY = "your-merchant-uuid-here"


Optional Settings

You can customize the behavior with these optional variables:

# Change which URLs are protected (Default: '/api/')
API_GATE_URL_PREFIX = "/v1/"

# Change cost per request (Default: 1 credit)
API_GATE_PRICE = 5


🛠️ Usage

For Your Users (The Consumers)

Once installed, any request to your protected URL prefix (e.g., /api/data) will require an API Gate Key in the header:

curl -H "X-Api-Key: customer-uuid-here" [https://yoursite.com/api/data](https://yoursite.com/api/data)


Error Responses

If a user is missing a key or runs out of credits, they receive a standard 402 Payment Required response:

{
    "error": "Insufficient Credits",
    "balance": 0,
    "top_up_url": "[https://api-gate-production.onrender.com/](https://api-gate-production.onrender.com/)"
}


🏗️ Architecture

This library operates as a Spoke in a Hub-and-Spoke financial model.

The Hub: The API Gate Hub (Ledger, Stripe Processing, User Accounts).

The Spoke: Your Django App (Product, Logic, Value).

When a request comes in, this middleware pings the Hub to verify funds and transfer credits from the Consumer to the Merchant.