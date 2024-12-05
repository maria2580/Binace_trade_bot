# Binance Automated Cryptocurrency Trading Bot

This project is a trading bot that automatically executes Bitcoin trades using Binance's Spot and Earn APIs. The bot collects and analyzes real-time data based on various investment strategies to automatically place buy and sell orders. It aims to continuously capture profitable trading opportunities while maximizing Earn interest.

## Key Features

- **Data Collection and Analysis**: Collects real-time price, RSI (Relative Strength Index), volume, and USDT APR data through Binance's WebSocket stream. In case of an error, uses REST API to back up data.
- **Multi-threaded Structure**: Executes each function (data collection, data monitoring, order execution, WebSocket connection) in independent threads to collect data and execute orders in real time.
- **Investment Strategy**: Implements buy/sell strategies based on APR, price drops, and RSI thresholds. For example, if the Bitcoin price drops by more than 5% in the last 24 hours, it triggers a buy order, and if the RSI exceeds 90, it triggers a sell order.
- **Earn Management**: Automatically subscribes USDT in the Earn wallet to Flexible Savings and supports transferring funds between Earn and Spot wallets for automated liquidity management.
- **Order Management**: After placing buy/sell orders, continuously monitors their status, and cancels and reissues unfulfilled orders after a certain period.

## Installation

1. Install the required libraries:

```
pip install pandas requests websocket-client pandas-ta
```

1. **Set API Key**:
    - Obtain `API_KEY` and `API_SECRET` from your Binance account and enter them in the code.
2. **Run the Program**:

```
python trading_bot.py
```

## APIs Used

- **WebSocket Stream**: Collects real-time price, volume, RSI, and APR data.
- **Market Data REST API**: Used for market data backup and indicator calculations.
- **Spot Trading API**: Issues buy/sell orders and manages orders.
- **Earn API**: Manages USDT Flexible Saving subscriptions and redemptions.
- **Wallet API**: Queries wallet balance and moves funds between Earn and Spot wallets.

## Global Variables Description

- `last_5_percent_buy_time`, `last_10_percent_buy_time`, `last_50_percent_buy_time`: Record the time when buy orders are triggered after a 5%, 10%, or 50% price drop, respectively, to prevent the same condition from triggering within 24 hours.
- **Data Queues**: Use queues to share and process price, RSI, volume, and APR data between threads.
- **Order Queue**: When conditions are met, stores buy/sell order information for the order execution thread to handle.

## Execution Flow

1. **Data Collection**: Real-time data is collected and added to queues through WebSocket.
2. **Data Monitoring**: Determines whether investment conditions are met and adds order data to the order queue.
3. **Order Execution**: Retrieves orders from the queue, executes buy/sell orders via the Binance API, and monitors fulfillment status.
4. **Earn Management**: After order execution, moves assets to the Earn wallet to maintain liquidity and maximize APR returns.

## Areas for Improvement and Additional Considerations

- **Retry Logic on Error**: Currently, errors are logged, and the program moves on. Adding retry logic for critical tasks (e.g., order placement, fund transfer) can improve reliability.
- **Enhanced Backup Data Usage Logic**: When a WebSocket error occurs, data is backed up using the REST API. Strengthening the logic to only use backup data after attempting WebSocket reconnection would be beneficial.
- **Buy and Sell Order Fulfillment Verification**: If an order is not fulfilled within 30 seconds, it is canceled and reissued. However, under certain market conditions, fulfillment issues may persist. Consider adjusting the price or changing order conditions to increase the likelihood of fulfillment.
- **Risk Management and Testing**: Before implementing the strategy in real trading, consider risk management measures. For example, backtesting and paper trading are important to verify the stability of the strategy.
- **API Call Frequency Management**: Binance API has rate limits, so adjusting the time between calls to avoid exceeding the rate limit is crucial. For example, optimize the monitoring thread's data polling frequency.

## Contribution

Pull requests to improve the project are always welcome. Feel free to submit code improvements, bug fixes, or new feature suggestions.

## License

This project is provided under the MIT License.
