# Car Deals Search MCP

> **Search used car listings from Cars.com, Autotrader, and KBB with AI assistants**

An MCP (Model Context Protocol) server that aggregates and searches car listings from multiple sources. Scrapes listings in parallel, extracts price, mileage, dealer info, and applies optional CARFAX-style filters (1-owner, no accidents, personal use).

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🚀 Quick Start

### Prerequisites

- **Node.js** (v16 or higher)
- **Chrome/Chromium** browser installed (required by Puppeteer)
  - If Chrome is not in the default location, set `PUPPETEER_EXECUTABLE_PATH` environment variable to point to your Chrome/Chromium binary

### Installation

```bash
# Clone the repository
git clone https://github.com/SiddarthaKoppaka/car_deals_search_mcp.git
cd car_deals_search_mcp

# Install dependencies (includes Puppeteer)
npm install
```

### Using with MCP Clients

Configure your MCP client (Claude Desktop, VS Code, GitHub Copilot, etc.) to use this server:

**For Claude Desktop** (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

```json
{
  "mcpServers": {
    "car-deals": {
      "command": "node",
      "args": ["/absolute/path/to/car_deals_search_mcp/src/server.js"]
    }
  }
}
```

**For other MCP clients**, refer to their documentation and use:
- **Command**: `node`
- **Args**: `["<absolute-path-to-repo>/src/server.js"]`

### Testing Standalone

```bash
# Run the test command
npm test

# Or test manually with a specific search
node -e "
const { scrapeCarscom } = require('./src/scraper.js');
scrapeCarscom({
  make: 'Toyota',
  model: 'Camry',
  oneOwner: true,
  noAccidents: true,
  personalUse: true
}, 5).then(listings => listings.forEach(l => console.log(l.format())));
"
```

---

## ✨ Features

- **Multi-source aggregation**: Search Cars.com, Autotrader, and KBB simultaneously
- **Smart filtering**: CARFAX-style filters (1-Owner, No Accidents, Personal Use)
- **Deal ratings**: Heuristic-based deal quality assessment
- **Parallel scraping**: Fast concurrent queries across sources
- **Stealth mode**: Puppeteer with anti-bot detection techniques

---

## 📊 Supported Sources

| Source     | Price | Mileage | Deal Rating | Dealer Info | CARFAX Filters |
|------------|:-----:|:-------:|:-----------:|:-----------:|:--------------:|
| Cars.com   | ✅    | ✅      | ✅          | ✅          | ✅             |
| Autotrader | ✅    | ✅      | ⚠️ Limited   | ✅          | ⚠️ Limited     |
| KBB        | ✅    | ✅      | ✅          | ⚠️ Limited   | ⚠️ Limited     |

---

## 🔧 MCP Tool: `search_car_deals`

### Parameters

| Parameter    | Type     | Required | Description |
|--------------|----------|----------|-------------|
| `make`       | string   | ✅       | Car manufacturer (e.g., "Toyota", "Honda") |
| `model`      | string   | ✅       | Car model (e.g., "Camry", "Accord") |
| `zip`        | string   | ❌       | ZIP code for local search (default: "90210") |
| `yearMin`    | integer  | ❌       | Minimum model year |
| `yearMax`    | integer  | ❌       | Maximum model year |
| `priceMax`   | integer  | ❌       | Maximum price in USD |
| `mileageMax` | integer  | ❌       | Maximum mileage |
| `maxResults` | integer  | ❌       | Max results per source (default: 10) |
| `sources`    | array    | ❌       | Sources to query: `["cars.com","autotrader","kbb"]` (default: all) |
| `oneOwner`   | boolean  | ❌       | Filter for CARFAX 1-owner vehicles only |
| `noAccidents`| boolean  | ❌       | Filter for no accidents reported |
| `personalUse`| boolean  | ❌       | Filter for personal use only (not rental/fleet) |

### Example Response

```
🚗 2021 Toyota Camry XSE
   💰 Price: $23,491
   📏 Mileage: 52,649 mi
   ⭐ Deal Rating: Good Deal
   🏆 CARFAX: 1-Owner | No Accidents | Personal Use
   🏪 Dealer: Valencia BMW
   🌐 Source: Cars.com
   🔗 https://www.cars.com/vehicledetail/...
```

---

## 🛠️ Technical Details

- **Scraping**: Puppeteer (headless Chromium) with stealth plugin to bypass bot detection
- **Concurrency**: Parallel scraper workers for simultaneous multi-source queries
- **Protocol**: Implements MCP (Model Context Protocol) for AI assistant integration
- **Data extraction**: Source-specific parsers normalize listings into a common schema

### Chrome/Chromium Requirement

This project uses Puppeteer, which requires Chrome or Chromium to be installed:

- **macOS**: Chrome is typically at `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- **Linux**: Usually auto-detected by Puppeteer or at `/usr/bin/chromium-browser`
- **Windows**: Typically at `C:\Program Files\Google\Chrome\Application\chrome.exe`

If Puppeteer cannot find your browser, set the environment variable:

```bash
export PUPPETEER_EXECUTABLE_PATH="/path/to/chrome"
```

---

## 🧪 Development & Testing

```bash
# Run tests
npm test

# Test individual scrapers
node src/scraper.js

# View code structure
ls -la src/
```

---

## 🤝 Contributing

Contributions are welcome! Please follow this workflow:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Add tests for new functionality
4. Commit your changes (`git commit -m 'Add amazing feature'`)
5. Push to the branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

Please include test coverage for scraping/parsing changes to avoid regressions when source sites update.

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details

---

## 🔗 Links

- **Repository**: https://github.com/SiddarthaKoppaka/car_deals_search_mcp
- **Issues**: https://github.com/SiddarthaKoppaka/car_deals_search_mcp/issues
- **MCP Protocol**: https://modelcontextprotocol.io
