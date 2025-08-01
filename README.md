# Prediction Market Frameworks

A unified Python SDK for prediction market platforms including Polymarket and Kalshi.

## Features

- 🚀 **Unified API** - Single interface for multiple prediction market platforms
- 🔒 **Type Safe** - Full type hints and Pydantic validation
- 🛡️ **Error Handling** - Comprehensive exception hierarchy with detailed context
- ⚙️ **Configurable** - Flexible configuration with environment variable support
- 🧪 **Well Tested** - Comprehensive test suite with >80% coverage
- 📚 **Well Documented** - Complete API documentation and examples

## Installation

```bash
# Install from source (development)
git clone https://github.com/your-org/prediction-market-frameworks
cd prediction-market-frameworks
pip install -e .

# Install with development dependencies
pip install -e ".[dev]"
```

## Quick Start

### Basic Usage

```python
from prediction_market_frameworks import PolymarketClient

# Initialize client (uses environment variables)
client = PolymarketClient()

# Get active events
events = client.get_active_events(limit=10)
for event in events:
    print(f"📅 {event.title}")

# Get market data
market = client.get_market("condition_id")
print(f"📊 {market.question}")
```

### Environment Variables

Set these environment variables for authentication:

```bash
export POLYMARKET_API_KEY="your_api_key"
export POLYMARKET_API_SECRET="your_api_secret"
export POLYMARKET_API_PASSPHRASE="your_passphrase"
export POLYMARKET_PRIVATE_KEY="your_private_key"

# Optional configuration
export POLYMARKET_TIMEOUT="30"
export POLYMARKET_MAX_RETRIES="3"
export POLYMARKET_DEFAULT_PAGE_SIZE="100"
```

### Advanced Configuration

```python
from prediction_market_frameworks import PolymarketClient, PolymarketConfig

# Custom configuration
config = PolymarketConfig(
    api_key="your_key",
    api_secret="your_secret",
    api_passphrase="your_passphrase",
    pk="your_private_key",
    timeout=60,
    max_retries=5,
    default_page_size=50,
    enable_auto_pagination=True,
)

client = PolymarketClient(config)

# Advanced users can access underlying clients directly
gamma_client = client.gamma  # Access to Gamma API
clob_client = client.clob    # Access to CLOB API
```

## Examples

### Get Market Data

```python
# Get active events
events = client.get_active_events()

# Get specific market
market = client.get_market("condition_id")

# Get order book
order_book = client.get_order_book("token_id")

# Get market statistics
stats = client.get_market_statistics("market_id")
```

### Trading Operations

```python
# Submit market order
result = client.submit_market_order(
    token_id="token_123",
    side="BUY",
    size=10.0
)

# Submit limit order
result = client.submit_limit_order_gtc(
    token_id="token_123",
    side="BUY",
    size=10.0,
    price=0.75
)

# Get open orders
orders = client.get_open_orders()

# Cancel order
client.cancel_order("order_id")
```

### Error Handling

```python
from prediction_market_frameworks import (
    PolymarketError,
    PolymarketRateLimitError,
    PolymarketAuthenticationError,
    PolymarketValidationError
)

try:
    events = client.get_events(limit=5000)  # Too large
except PolymarketValidationError as e:
    print(f"Validation error: {e}")
    print(f"Field: {e.field}, Value: {e.value}")
except PolymarketRateLimitError as e:
    print(f"Rate limited! Retry after {e.retry_after} seconds")
except PolymarketAuthenticationError:
    print("Check your API credentials")
except PolymarketError as e:
    print(f"General error: {e}")
```

## API Reference

### Main Classes

- **`PolymarketClient`** - Main unified client
- **`PolymarketConfig`** - Configuration management

### Exception Hierarchy

- **`PolymarketError`** - Base exception
  - **`PolymarketAPIError`** - API-related errors
    - **`PolymarketAuthenticationError`** - Auth failures
    - **`PolymarketRateLimitError`** - Rate limiting
    - **`PolymarketNotFoundError`** - Resource not found
  - **`PolymarketValidationError`** - Validation errors
  - **`PolymarketNetworkError`** - Network/connection issues

## Development

### Setup

```bash
# Clone repository
git clone https://github.com/your-org/prediction-market-frameworks
cd prediction-market-frameworks

# Install with development dependencies
pip install -e ".[dev]"

# Install pre-commit hooks
pre-commit install
```

### Testing

```bash
# Run tests
pytest

# Run tests with coverage
pytest --cov=src --cov-report=html

# Run specific test file
pytest tests/test_polymarket_client.py

# Run with markers
pytest -m "not slow"
```

### Code Quality

```bash
# Format code
black src/ tests/

# Lint code
ruff check src/ tests/

# Type checking
mypy src/
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Add tests for your changes
5. Ensure all tests pass (`pytest`)
6. Commit your changes (`git commit -m 'Add amazing feature'`)
7. Push to the branch (`git push origin feature/amazing-feature`)
8. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Roadmap

- [x] Polymarket integration
- [ ] Kalshi integration
- [ ] Async client support
- [ ] WebSocket streaming
- [ ] Advanced order types
- [ ] Portfolio management tools
