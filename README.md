                           Decribing the architecture of this Project
                           
Engine
The Orderbook class simulates a limit order book, a system used by exchanges to match buy and sell orders based on price and quantity. Here’s how the key operations work:

Adding Orders:
When an order is added (addOrder), the system attempts to match it with existing orders of the opposite side.
For a buy order, it looks for asks with a price less than or equal to the buy order’s price.
For a sell order, it looks for bids with a price greater than or equal to the sell order’s price.
If matches are found, trades (fills) are created, and the orderbook is updated by removing fully filled orders and updating the filled quantities.
Unfilled or partially filled orders are added to the bids or asks array.
Matching Logic:
The matching process follows a price-time priority (implied by the order of the bids and asks arrays).
For each match, a Fill object is created to record the trade details.
The lastTradeId is incremented to ensure unique trade IDs.
Depth Calculation:
The getDepth method aggregates orders by price to provide a view of the market depth, which is useful for displaying the orderbook to users (e.g., in a trading interface).
Order Cancellation:
Orders can be canceled using cancelBid or cancelAsk, which remove the order from the respective array.
Potential Improvements
Self-Trade Prevention:
The code includes a TODO for preventing self-trades (when a user’s buy order matches their own sell order). This can be implemented by checking if order.userId === this.asks[i].userId in matchBid (and similarly in matchAsk) and skipping the match.
Optimizing getDepth:
The getDepth method recomputes the entire depth, which can be slow for large orderbooks. Instead, the depth could be maintained incrementally:
Update bidsObj and asksObj when orders are added, matched, or canceled.
Store bids and asks as sorted data structures (e.g., a map or sorted array) to avoid re-aggregation.
Sorting Orders:
The code does not explicitly sort bids (highest price first) or asks (lowest price first), which is typical for orderbooks to ensure price-time priority. Sorting should be added when orders are inserted.
Performance:
Use more efficient data structures (e.g., a priority queue or red-black tree) for bids and asks to improve matching and depth calculation performance.
Avoid array splicing in loops (e.g., in matchBid and matchAsk) by marking orders for deletion and removing them after the loop.
Error Handling:
Add validation for order inputs (e.g., positive price/quantity, valid orderId).
Handle edge cases like empty orderbooks or invalid cancellations.
Price Precision:
The Fill interface converts prices to strings (price: string), which may be for formatting purposes. Ensure consistent handling of price precision to avoid floating-point errors.
