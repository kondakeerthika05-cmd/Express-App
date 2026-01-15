# Express-App
Express App with Core Node.js Modules
1️⃣ Initialize Node.js Project
npm init -y


This creates the default package.json.

2️⃣ Install Express
npm i express

3️⃣ Project Structure
express-modules-app/
│
├── Data.txt
├── index.js
├── read.js
├── package.json
└── node_modules/

4️⃣ Create Data.txt

Data.txt

This is some sample data inside Data.txt.

5️⃣ Create File Reading Module (read.js)

This module uses the fs module and exports a function.

read.js

const fs = require("fs");
const path = require("path");

const readFileData = () => {
  return new Promise((resolve, reject) => {
    const filePath = path.join(__dirname, "Data.txt");

    fs.readFile(filePath, "utf-8", (err, data) => {
      if (err) {
        reject("Error reading the file");
      } else {
        resolve(data);
      }
    });
  });
};

module.exports = readFileData;


✔ Uses Promise for clean async handling
✔ Includes basic error handling (Bonus)

6️⃣ Create Express Server (index.js)

index.js

const express = require("express");
const os = require("os");
const dns = require("dns");

const readFileData = require("./read");

const app = express();
const PORT = 3000;

// Test Route
app.get("/test", (req, res) => {
  res.send("Test route is working!");
});

// Read File Route
app.get("/readfile", async (req, res) => {
  try {
    const data = await readFileData();
    res.send(data);
  } catch (error) {
    res.status(500).send({ error: error });
  }
});

// System Details Route
app.get("/systemdetails", (req, res) => {
  const totalMemory = (os.totalmem() / 1024 / 1024 / 1024).toFixed(2);
  const freeMemory = (os.freemem() / 1024 / 1024 / 1024).toFixed(2);

  const systemDetails = {
    platform: os.platform(),
    totalMemory: `${totalMemory} GB`,
    freeMemory: `${freeMemory} GB`,
    cpuModel: os.cpus()[0].model,
    cpuCores: os.cpus().length // Bonus
  };

  res.json(systemDetails);
});

// Get IP Route (IPv4 + IPv6)
app.get("/getip", (req, res) => {
  dns.lookup("masaischool.com", { all: true }, (err, addresses) => {
    if (err) {
      res.status(500).json({ error: "DNS lookup failed" });
    } else {
      res.json({
        hostname: "masaischool.com",
        addresses: addresses
      });
    }
  });
});

// Start Server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});

7️⃣ Run the Server
node index.js


⚠️ Important:
Whenever you change the code, restart the server manually, otherwise updates won’t reflect.

8️⃣ Expected Outputs (Browser / Postman)
🔹 GET /test
Test route is working!

🔹 GET /readfile
This is some sample data inside Data.txt.

🔹 GET /systemdetails
{
  "platform": "win32",
  "totalMemory": "16.00 GB",
  "freeMemory": "8.20 GB",
  "cpuModel": "Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz",
  "cpuCores": 12
}

🔹 GET /getip
{
  "hostname": "masaischool.com",
  "addresses": [
    { "address": "3.7.30.29", "family": 4 },
    { "address": "2406:da1c:5c1:5200::1", "family": 6 }
  ]
}
