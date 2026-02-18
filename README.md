# Bitcoin Profit Target Calculator
<img width="500" height="1057" alt="SCR-20260218-ovfk" src="https://github.com/user-attachments/assets/27fe713d-c42e-4cf6-8c5e-ed21a1c42986" />


A simple, elegant web-based calculator that helps Bitcoin investors determine the target BTC price needed to achieve their desired profit.

![Bitcoin Calculator](https://img.shields.io/badge/Bitcoin-Calculator-F7931A?style=for-the-badge&logo=bitcoin&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Demo](#demo)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Input Fields Explained](#input-fields-explained)
- [Example Calculations](#example-calculations)
- [Technical Details](#technical-details)
- [Browser Support](#browser-support)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)

## Overview

This Bitcoin Profit Target Calculator is a standalone, single-file web application designed for Bitcoin investors who want to quickly determine what price BTC needs to reach to achieve their profit goals. Whether you're a long-term holder or an active trader, this tool simplifies the math behind profit calculations.

The calculator features a modern, bold design with a neo-brutalist aesthetic, making it both functional and visually appealing. No server-side processing, no data collection – everything runs locally in your browser.

## Features

- **Profit Calculation in USD or Percentage**: Choose to calculate based on a specific dollar amount or a percentage return
- **Real-Time Calculation**: Instant results without page reloads
- **Break-Even Price Display**: See your break-even point at a glance
- **Comprehensive Output**: Get detailed information including total investment cost, profit percentage, and target sell value
- **Responsive Design**: Works seamlessly on desktop, tablet, and mobile devices
- **No Dependencies**: Single HTML file with embedded CSS and JavaScript
- **Privacy-Focused**: All calculations happen locally in your browser
- **Accessible**: Built with ARIA attributes for screen reader compatibility

## Demo

To see the calculator in action, simply open the `index.html` file in any modern web browser.

### Screenshot

```
┌─────────────────────────────────────────────┐
│   BITCOIN TARGET PRICE & PROFIT CALCULATOR  │
├─────────────────────────────────────────────┤
│                                             │
│  Total Bitcoins Owned (BTC):                │
│  ┌─────────────────────────────────────┐    │
│  │ 1.5                              │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  Average Buy Price per BTC ($):             │
│  ┌─────────────────────────────────────┐    │
│  │ 80000                            │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  Desired Profit ($):                        │
│  ┌─────────────────────────────────────┐    │
│  │ 20000                            │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │      CALCULATE TARGET PRICE         │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ✅ Target BTC Price Needed: $93,333.33     │
│                                             │
└─────────────────────────────────────────────┘
```

## Installation

### Option 1: Direct Download

1. Download or clone this repository:
   ```bash
   git clone https://github.com/yourusername/btc-dream.git
   ```

2. Navigate to the project directory:
   ```bash
   cd btc-dream
   ```

3. Open `index.html` in your web browser:
   ```bash
   # On macOS
   open index.html
   
   # On Linux
   xdg-open index.html
   
   # On Windows
   start index.html
   ```

### Option 2: Using a Local Server

For development or if you prefer using a local server:

```bash
# Using Python 3
python -m http.server 8000

# Using Node.js (with http-server installed)
npx http-server

# Using PHP
php -S localhost:8000
```

Then open `http://localhost:8000` in your browser.

### Option 3: Direct Browser Opening

Simply double-click the `index.html` file, and it will open in your default web browser.

## Usage

### Step-by-Step Guide

1. **Enter Your BTC Holdings**: Input the total amount of Bitcoin you own (up to 6 decimal places for precision)

2. **Enter Your Average Buy Price**: Input the average price you paid per BTC in USD

3. **Enter Your Desired Profit**: Choose ONE of the following:
   - **Profit in USD**: Enter the dollar amount of profit you want to make
   - **Profit Percentage**: Enter the percentage return you're targeting

4. **Click "Calculate Target Price"**: The calculator will display your results instantly

### Understanding the Results

The calculator provides the following information:

| Field | Description |
|-------|-------------|
| **Target BTC Price** | The price BTC needs to reach for your profit goal |
| **Total BTC Owned** | Your Bitcoin holdings (for verification) |
| **Average Buy Price** | Your cost basis per BTC |
| **Break-even BTC Price** | The price at which you neither gain nor lose |
| **Total Investment Cost** | Your total capital invested |
| **Desired Profit** | Your profit target in USD |
| **Profit Percentage** | Your profit as a percentage of investment |
| **Total Sell Value Needed** | The total value you need to sell at for your target |

## How It Works

### The Formula

The calculator uses the following formula:

```
Total Cost = BTC Owned × Average Buy Price

Desired Profit (from %) = Total Cost × (Profit Percentage / 100)

Target Sell Value = Total Cost + Desired Profit

Target BTC Price = Target Sell Value / BTC Owned
```

### Example Calculation Flow

For someone who owns 1.5 BTC bought at an average of $80,000:

1. **Total Investment**: 1.5 × $80,000 = $120,000
2. **Desired Profit**: $20,000 (or ~16.67%)
3. **Target Sell Value**: $120,000 + $20,000 = $140,000
4. **Target BTC Price**: $140,000 ÷ 1.5 = $93,333.33

## Input Fields Explained

### Total Bitcoins Owned (BTC)

- **Type**: Decimal number
- **Precision**: Up to 6 decimal places
- **Range**: 0 to 1,000,000 BTC
- **Example**: `1.5`, `0.042`, `21.123456`

> This represents your total Bitcoin holdings. If you've made multiple purchases at different prices, enter the sum of all your BTC here.

### Average Buy Price per BTC ($)

- **Type**: Decimal number
- **Precision**: Up to 2 decimal places
- **Range**: $1 to $1,000,000
- **Example**: `80000`, `45000.50`, `100000`

> This is your cost basis per Bitcoin. If you bought at different prices, calculate the weighted average:
> ```
> Average Price = Total Spent ÷ Total BTC Owned
> ```

### Desired Profit ($)

- **Type**: Decimal number
- **Precision**: Up to 2 decimal places
- **Range**: $0 to $1,000,000
- **Example**: `20000`, `5000.50`, `100000`

> Enter the absolute dollar profit you want to make. Leave blank if using percentage.

### Desired Profit (%)

- **Type**: Decimal number
- **Precision**: Up to 2 decimal places
- **Range**: 0% to 1,000%
- **Example**: `25`, `50.5`, `100`

> Enter your target return as a percentage of your investment. Leave blank if using dollar amount.

## Example Calculations

### Example 1: Dollar-Based Profit Target

| Input | Value |
|-------|-------|
| BTC Owned | 0.5 BTC |
| Average Buy Price | $65,000 |
| Desired Profit ($) | $10,000 |

**Results:**
- Target BTC Price: $85,000.00
- Total Investment: $32,500
- Profit Percentage: 30.77%

### Example 2: Percentage-Based Profit Target

| Input | Value |
|-------|-------|
| BTC Owned | 2 BTC |
| Average Buy Price | $70,000 |
| Desired Profit (%) | 50% |

**Results:**
- Target BTC Price: $105,000.00
- Total Investment: $140,000
- Desired Profit: $70,000

### Example 3: Small Holdings

| Input | Value |
|-------|-------|
| BTC Owned | 0.015 BTC |
| Average Buy Price | $95,000 |
| Desired Profit ($) | $500 |

**Results:**
- Target BTC Price: $128,333.33
- Total Investment: $1,425
- Profit Percentage: 35.09%

## Technical Details

### File Structure

```
btc-dream/
├── index.html      # Main application (single file)
├── README.md       # Documentation
└── telemetry-id    # Internal tracking file
```

### Technologies Used

- **HTML5**: Semantic markup with ARIA accessibility attributes
- **CSS3**: Custom properties, flexbox, animations, and responsive design
- **Vanilla JavaScript**: No frameworks or libraries required

### Key Features Implementation

#### Input Validation
```javascript
// Built-in HTML5 validation
min="0"
max="1000000"
step="0.0001"
pattern="^\\d+(\\.\\d{1,6})?$"
```

#### Calculation Logic
```javascript
const totalCost = btcOwned * avgPrice;
const desiredProfitValue = profitUSD > 0 ? profitUSD : totalCost * (profitPercent / 100);
const targetBTCPrice = (totalCost + desiredProfitValue) / btcOwned;
```

### Design System

The application uses a neo-brutalist design aesthetic:

| Element | Style |
|---------|-------|
| Background | Warm cream (#f0e6d3) |
| Container | White with bold black borders |
| Shadows | Offset hard shadows (8px 8px 0 #000) |
| Accent Colors | Yellow (#fef08a), Green (#d1fae5), Red (#fca5a5), Cyan (#a5f3fc) |
| Typography | Inter font family, uppercase headings |
| Interactions | Transform animations on hover/focus |

## Browser Support

| Browser | Supported |
|---------|-----------|
| Chrome | ✅ Latest 2 versions |
| Firefox | ✅ Latest 2 versions |
| Safari | ✅ Latest 2 versions |
| Edge | ✅ Latest 2 versions |
| Opera | ✅ Latest 2 versions |
| IE | ❌ Not supported |

### Minimum Requirements

- CSS Flexbox support
- ES6 JavaScript support
- CSS @import for Google Fonts

## Customization

### Changing Colors

Edit the CSS variables in the `<style>` section:

```css
body {
  background: #f0e6d3;  /* Change background color */
}

.container {
  border: 4px solid #000;  /* Change border color */
  box-shadow: 8px 8px 0 #000;  /* Change shadow color */
}

button {
  background: #fef08a;  /* Change button color */
}
```

### Modifying Input Limits

Adjust the `min` and `max` attributes in the HTML:

```html
<input type="number" id="btcOwned" min="0" max="1000000" ... />
```

### Adding New Features

The modular JavaScript function makes it easy to extend:

```javascript
function calculate() {
  // Add your custom logic here
  // Access values with: document.getElementById("elementId").value
}
```

## Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit your changes**: `git commit -m 'Add amazing feature'`
4. **Push to the branch**: `git push origin feature/amazing-feature`
5. **Open a Pull Request**

### Ideas for Contributions

- Add dark mode toggle
- Implement multiple cryptocurrency support
- Add historical price data integration
- Create a "save calculation" feature
- Add chart visualization
- Implement currency conversion

## Security & Privacy

- **No Data Collection**: All calculations happen client-side
- **No Cookies**: The application doesn't use cookies or local storage
- **No External APIs**: No data is sent to external servers
- **Open Source**: Code is fully auditable

## Disclaimer

This calculator is for educational and informational purposes only. It does not constitute financial advice. Cryptocurrency investments carry significant risks, and past performance does not guarantee future results. Always conduct your own research and consult with a qualified financial advisor before making investment decisions.

## License

This project is open source and available under the [MIT License](LICENSE).

---

**Made with ❤️ for the Bitcoin community**

*Remember: Not your keys, not your coins. HODL responsibly.*
