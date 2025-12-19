# 🪙 CryptoTop10 - Real-Time Cryptocurrency Tracker

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Netlify](https://img.shields.io/badge/Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)
![Render](https://img.shields.io/badge/Render-46E3B7?style=for-the-badge&logo=render&logoColor=white)

A full-stack cryptocurrency dashboard that tracks the top 10 coins using the **CoinGecko API**. It features automated background data collection, historical tracking, and a clean, responsive UI.

## 🚀 Features

* **Real-Time Dashboard:** Displays the current price, market cap, and changes for the top 10 cryptocurrencies.
* **Automated Data Fetching:** Backend cron jobs automatically fetch data to keep the database fresh without user intervention.
* **Historical Data Tracking:** Archives price data hourly to visualize trends over time.
* **Manual Snapshot:** Users can trigger a manual save of current prices to the history log.
* **Coin History View:** View detailed historical data for specific coins.

## 🛠️ Tech Stack

### Frontend (Client)
* **Framework:** React (Vite)
* **Styling:** Tailwind CSS
* **HTTP Client:** Axios
* **Deployment:** Netlify

### Backend (Server)
* **Runtime:** Node.js & Express.js
* **Database:** MongoDB (Atlas)
* **ODM:** Mongoose
* **Automation:** `node-cron` (Task Scheduling)
* **External API:** CoinGecko
* **Deployment:** Render

## ⚙️ Backend Automation (Cron Jobs)

The backend utilizes `node-cron` to manage data consistency and history automatically.

| Frequency | Cron Expression | Function | Logic |
| :--- | :--- | :--- | :--- |
| **Every 30 Mins** | `*/30 * * * *` | **Refresh Dashboard** | Fetches fresh data from CoinGecko, **deletes** the old `CurrentData`, and inserts the new batch. This ensures the main view is always up-to-date. |
| **Every 60 Mins** | `0 * * * *` | **Archive History** | Fetches fresh data and **appends** it to the `HistoryData` collection. This creates a permanent log for analyzing price trends over time. |

**Snippet from `scheduler.js`:**
```javascript
// 30 Mins schedule - Refreshes "Current" View
cron.schedule('*/30 * * * *', async () => {
    const coinData = await fetchTopTenCoins();
    if(coinData){
        await CurrentData.deleteMany({}); // Wipe old data
        await CurrentData.insertMany(coinData); // Insert new
    }
});
