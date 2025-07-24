
![JPEG](https://github.com/user-attachments/assets/68ba69b9-75e0-42cd-ac86-fba4449e55e7)
Backend architrcture
![2](https://github.com/user-attachments/assets/198892c2-6e17-49f2-baf9-cdcf9af440a6)
API - An API Server the user sends HTTP requests to
Engine - Runs various market orderbooks, stores user balances in memory
Websocket - Websocket server that user subscribes to real time events from
DB Processor - Processes messages from the Engine and persists them in the DB
Frontend - NextJS app (same as last week, only the URLs would change)
Market maker (mm) - Places random orders to keep the book liquid
Redis - Queue and pub sub
TimescaleDB - Creates buckets of klines based on price feed
