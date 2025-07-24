
## Engine
Trading System Overview
This repository implements a trading system with two core components: Orderbook.ts and Engine.ts. Below is a comparison of their roles, functionalities, and interactions.
Orderbook.ts vs. Engine.ts
Role in the System

Orderbook.ts: Manages a single trading pair (e.g., TATA_INR), handling order matching, bids, asks, and market depth.
Engine.ts: Coordinates the entire trading system, managing multiple order books, user balances, and client interactions.

Key Functionalities



Aspect
Orderbook.ts
Engine.ts



Scope
Manages a single market’s order book (bids and asks).
Oversees multiple markets, user balances, and system-wide operations.


Order Management
Adds, matches, and cancels orders; generates Fill objects for trades.
Delegates order operations to Orderbook, manages funds, and publishes updates.


Balance Management
Does not handle balances; focuses on orders and trades.
Tracks user balances (available and locked) for all assets.


State Persistence
Provides getSnapshot for order book state.
Saves/loads system-wide snapshots to snapshot.json every 3 seconds.


Client Interaction
No direct client interaction; called by Engine.
Processes client messages and sends responses via RedisManager.


Error Handling
Minimal; assumes valid inputs.
Catches errors, logs them, and notifies clients (e.g., ORDER_CANCELLED).


Extensibility
Handles one trading pair; multiple instances for multiple markets.
Manages multiple Orderbook instances for system scalability.


Performance Considerations

Orderbook.ts: 
The getDepth method could be optimized (TODO) to avoid recomputing depth for large order books.
Order matching (matchBid/matchAsk) is straightforward but may benefit from sorted orders.


Engine.ts: 
Periodic snapshot saving via fs.writeFileSync may be a bottleneck in high-throughput systems.
Relies on RedisManager for real-time messaging.



Notable TODOs

Orderbook.ts:
Implement self-trade prevention to avoid matching a user’s own buy/sell orders.
Optimize getDepth by maintaining aggregated depth during order matching.


Engine.ts:
Use a decimal library to avoid floating-point precision issues.
Replace Math.random() for orderId generation to prevent collisions.



Relationship

Dependency: Engine.ts creates and manages Orderbook instances; Orderbook.ts uses BASE_CURRENCY from Engine.ts.
Interaction: Engine calls Orderbook methods (e.g., addOrder, getDepth) and handles balance updates, persistence, and client communication.
Separation of Concerns:
Orderbook: Focuses on order matching and market state for a single trading pair.
Engine: Coordinates user funds, multiple markets, and external systems.



Example Workflow
When a client sends a CREATE_ORDER to buy 10 TATA at 100 INR:

Engine:
Processes the message via process.
Calls createOrder to validate funds, lock 1000 INR, and create an Order.
Passes the order to Orderbook.addOrder.


Orderbook:
Matches the buy order against asks via matchBid.
Updates bids/asks and returns executedQty and fills.


Engine:
Updates balances based on fills.
Publishes trade/order updates to Redis.
Sends an ORDER_PLACED response to the client.
Saves system state to snapshot.json.



This modular design ensures scalability and maintainability, with Orderbook handling market-specific logic and Engine managing system-wide operations.
