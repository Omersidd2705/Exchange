# Orderbook Trading System

This project implements a limit order book system for matching buy and sell orders in a trading environment, simulating the functionality used by financial exchanges. The `Orderbook` class is the core component, managing orders for a specific trading pair (e.g., BTC/USD).

## Orderbook Functionality

The `Orderbook` class handles the following key operations:

- **Adding Orders**:
  - Orders are added via the `addOrder` method, which attempts to match them with existing orders of the opposite side.
  - Buy orders match with asks at a price less than or equal to the buy order’s price.
  - Sell orders match with bids at a price greater than or equal to the sell order’s price.
  - Matches generate trades (fills), and the orderbook updates by removing fully filled orders and tracking filled quantities.
  - Unfilled or partially filled orders are added to the `bids` or `asks` array.

- **Matching Logic**:
  - Follows price-time priority, implied by the order of the `bids` and `asks` arrays.
  - Creates a `Fill` object for each match, recording trade details (price, quantity, trade ID, etc.).
  - Increments `lastTradeId` to ensure unique trade IDs.

- **Depth Calculation**:
  - The `getDepth` method aggregates orders by price to provide market depth, useful for displaying the orderbook in a trading interface.

- **Order Cancellation**:
  - Orders can be canceled using `cancelBid` or `cancelAsk
