# Binance-Counter-Trade-Bot with DCA



# 1. Introduction
**Welcome to my Binance Counter-Trade Bot!** 

As someone who has been counter trading for a long time using 3commas bot, I was concerned about the security of my personal information after the recent 3commas incident. 
As a result, I decided to build and host my own trading bot for increased security and peace of mind.

This project was developed to allow me to trade on my own terms without having to share my exchange API with any third-party service. With this bot, I can execute trades on various cryptocurrency exchanges based on my own set of trading rules and criteria.

It's worth noting that the bot also uses the **DCA (Dollar Cost Averaging)** strategy same way 3commas DCA BOT works .

In this documentation, I will provide you with a comprehensive guide on how to install and use the bot, including an overview of its features and functionality. Let's get started!

# 2. Getting Started
To get started with the trading bot, you will need a VPS (Virtual Private Server) to host the bot. The bot is designed to run on a VPS, which provides a secure and stable environment for the bot to operate in.

Once you have access to a VPS, **you can follow these steps to install and run the bot**:

1. Clone the project's GitHub repository to your VPS using the git clone command.
**git clone https://github.com/TZacksEG/Binance-Counter-Trade-Bot**
2. Navigate to the <**Binance-Counter-Trade-Bot**> directory.
3. Install the required Python packages by running **`pip install -r requirements.txt`**
4. run the following command to create main.ini file ( Config file ) **`python3 main.py`**
5. Open main.ini and fill in your API keys, trading rules, and other settings in the file.
6. Run the bot by executing the main.py file in the terminal using the python3 main.py command.

Once you see **`WARNING - WebSocket connected`** in the terminal 
press `CRTL+Z` to stop the terminal from running the bot as there are more few steps to do .

7. Navigate to the **service** directory, **edit** the **binance-bot.service** and your **watcher.service**
Change the text in the red boxes to your bot path in your VPS .
Change user to your VPS username

![image](https://user-images.githubusercontent.com/106902748/219901461-89bf26b9-7553-4c0d-95b9-ccfb35149a19.png)

once you finished editing and saved the files run these commands in terminal and make sure your current working directory is **service**  

to navigate to it in the terminal run the following command
`cd Binance-Counter-Trade-Bot/service`

and then run the following commands for each file in row -- **its better to POSTPONE THIS STEP  till you set the TA conditions in the main.ini file** .

`sudo cp binance-bot.service /etc/systemd/system/`

`sudo systemctl enable binance-bot.service`

`sudo systemctl restart binance-bot.service`

`sudo systemctl status binance-bot.service`


**This will run the BOT in the background , it will automatically execute trades on your behalf based on the rules and settings you specified in the main.ini file..**


Please note that the above steps are simplified for the purpose of this documentation, and you may need to adjust the instructions based on your specific VPS setup and operating system. If you need further assistance with the installation process, feel free to reach out to me at **https://discord.gg/RWtT7Nx9jh**

# 3. Usage
The trading bot is designed to automatically execute trades on the Binance exchange based on a specific strategy and trading rules.

The bot's strategy is based on a "Counter trade " signal, which is triggered when a short position is getting liquidated. 
However, to refine the strategy and reduce the risk of false positives, the bot is programmed to check a number of technical analysis (TA) conditions before executing a trade based on the signal.
including the following :
- **Liquidation value  minimal.**
- **BTC VWAP.**
- **VWAP** 
- **Bollinger Bands**
- **Parabolic SAR**
- **Volume**

**The TA conditions and other trading rules can be adjusted and configured by modifying the main.ini file**

This allows you to tailor the bot's behavior to their specific strategy settings and risk tolerance.

**once the conditions rules are TRUE** , the BOT Passes the signal as a trade to Binance and DCA orders are created along with TP 

It is important to note that while the bot is designed to operate autonomously.

it is still subject to market conditions and other factors that can impact the performance of the trading strategy. 
Therefore, it is recommended that you monitor the bot's performance and adjust the trading rules as necessary to maximize returns and minimize risk.

It is important to carefully consider your investment strategy and risk tolerance before using the trading bot, and to be prepared to take necessary actions in case of unexpected situations or errors.

# 4. Config file (main.ini) and condition settings

![image](https://user-images.githubusercontent.com/106902748/219903114-3bd88976-4357-4f6d-8863-cf358ff3667c.png)

[settings]

### Leave these values the way its .

> **timezone** = Africa/Cairo

> **debug** = False

> **logrotate** = 7

> **database-name** = binance 


### Your BOT DCA Settings .

> **base_order** = 50

> **safety_order** = 50

> **safety_order_step_scale** = 1.25

> **safety_order_volume_scale** = 1.5

> **max_safety_orders** = 4

> **price_deviation** = 2.25

### Your TP Settings .
I have developed a bot that allows you to set different take-profit targets for each of your DCA orders. This means that you can increase/lower TP targets based on market conditions and chart analysis.

**Please note** that the bot automatically calculates Binance fees, which are 0.2% for the base order and 0.3% for DCA orders. The fees are added to the final take-profit target, so if the user sets a take-profit target of 1% while still in the base order, the bot will automatically set a take-profit target of 1.2%

> **tp** = 1.0

> **tp0** = 1.5

> **tp1** = 2.0

> **tp2** = 2.5

> **tp3** = 3.0

> **tp4** = 3.1

> **tp5** = 3.2

### Your Binance API .

**Make Sure you whitelist your API to your server IP .. DONT LEAVE IT WITHOUT WHITELISTING YOUR SERVER IP**.

> **apikey** = Your Binance API Key

> **apisecret** = Your Binance API Secret
********
### Conditions .
**liquidation-minimal** = 600   -- minimal trade liquidation value to open deals

**volume** = 5000000    -- minimal 24h Trading Volume for any coin
****
**vwap** = 2.0    -- VWAP Tradable Green ZONE .. a way to make sure your trades are in safe ( tradable zone ) not too high not too low .

Tradable zone is when price is within the 2 lines .. not below it , not above it .

To know more about how to set this value .. i have created a TradingView indicator for you .. 

![image](https://user-images.githubusercontent.com/106902748/219905536-b2301993-54d8-4c57-b84c-a294acfb18cc.png)

You need to decide the tradable zone within the two lines for your longs . 

[Vwap GreenZone indcatorlink](https://www.tradingview.com/script/JCh3AXPW-ZCrypto-Vwap-Green-Zone/) 

****
**bb** = 20   -- KEEP THIS AT 20 , or **change if you know what you doing**

**stddev** = 2.5 --- BB Condition is designed to not buy wicks .. we dont want to buy at any price that is outside the BB upper band. Staying inside the BB Band gives you better trades . a bit safer than buying at higher price than upper band of BB 

![image](https://user-images.githubusercontent.com/106902748/219905998-6737e1d0-b643-4750-8c62-c7c087b9c1c9.png)

to change this value add BB indicator in your TV chart and change this value 

![image](https://user-images.githubusercontent.com/106902748/219906058-dcd9cfd0-3531-4bcf-b0b6-f59013cd2d8f.png)

****
**psar-acceleration** = 0.02 -- The PSAR is pretty simple condition if price is upper than PSAR then all is good , otherwise its a BIG NO, **I would recommend leaving this value as its , or change if you know what you doing**

![image](https://user-images.githubusercontent.com/106902748/219906189-a3e49458-8952-4d24-bd2d-4a123e0b1d1f.png)


********
**[notifier]**     --- pretty simple .. if you want to receive notification for your bot in Telegram , just set the telegram to True , same goes to discord and make sure you add the required data 
  
> **telegram** = False

> **telegram-bot-token** = <TOKEN>

> **telegram-userid** = <userid>

> **discord** = True

> **discord-webhook** = 157456546245454696/Jf24vHTl-_ioev8UJB3egDDAdfg56DDXjP-E-4w5VkNFDQuRFWSSQEDXQPkK3D7
**For discord : add whats after "https://discord.com/api/webhooks/" but dont add https://discord.com/api/webhooks/



# 5. Features
- **Customizable trading rules**: The bot's behavior can be tailored to your specific trading strategy and risk tolerance by modifying the settings in the main.ini file.
- **Dollar Cost Averaging (DCA) strategy**: The bot uses the DCA strategy to automatically execute trades and spread out the risk over multiple orders.
- **Technical Analysis (TA) conditions**: The bot checks a number of TA conditions, including liquidation value, BTC VWAP, VWAP, Bollinger Bands, Parabolic SAR, and volume, before executing a trade based on the Counter-Trade signal.
- A**utomatic order creation**: Once the TA conditions and other trading rules are met, the bot passes the signal as a trade to Binance and DCA orders are created along with TP.
- **User-friendly configuration**: The bot is easy to install and configure, and the main.ini file is user-friendly.
-  **Backtesting**: You can test your trading rules and strategy using historical data to see how the bot would perform in a specific market condition.
- **Notifications**: The bot can send notifications via email or Telegram when a trade is executed, a DCA order is created, or other important events occur.
- **Secure and reliable**: Hosting the bot on a VPS provides a secure and stable environment for the bot to operate in. The bot is also designed to be reliable and able to handle unexpected errors and situations.

- **User Interface** for Monitoring Trading Activity -  The UI displays important information such as current deals and their status, profit and loss (PNL), average price, number of DCA filled, total bot profit, total amount invested, and more.

![Screenshot_3](https://user-images.githubusercontent.com/106902748/219907862-c1e484d8-aea9-4e1f-b840-b657aa5ae968.png)

****
**Disclaimer**: Trading cryptocurrencies can be highly volatile and carries significant risk. 
 
 The information and advice provided by this bot is for informational and educational purposes only and should not be considered investment advice. 
 
 Any decision to buy, sell, or hold cryptocurrency is the sole responsibility of the user and should be based on their own research and analysis.
  The bot does not make any guarantees as to the accuracy or reliability of its information and advice, and shall not be held liable for any losses incurred as a result of using its services. 
  
**By using this bot, you acknowledge and accept that you are solely responsible for any investment decisions you make, and that you do so at your own risk.**



