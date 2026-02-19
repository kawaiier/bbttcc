# Specification: Live BTC Price Display & Comparison Feature

## 1. Feature Overview

This specification outlines the addition of a live Bitcoin price display and comparison feature to the existing Bitcoin Profit Target Calculator. The feature will:

- Display the current live BTC price from CoinGecko's free API
- Show a timestamp indicating when the price was last updated
- Auto-refresh the price every 60 seconds
- Compare the calculated target price to the current live price
- Provide visual indicators showing whether the target is reachable

---

## 2. API Selection

### Provider: CoinGecko (Free API - No API Key Required)

**Endpoint:**
```
https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd
```

**Response Format:**
```json
{
  "bitcoin": {
    "usd": 97500.00
  }
}
```

**Rate Limits:**
- 10-30 calls/minute (free tier)
- Ideal for 60-second refresh intervals

**Implementation Notes:**
- Use `fetch()` API for network requests
- No authentication headers required
- Handle CORS via standard fetch (CoinGecko supports CORS)

---

## 3. UI/UX Changes

### 3.1 Live Price Indicator Section

**Location:** Top of the calculator, immediately after the `<h1>` title

**HTML Structure:**
```html
<div id="livePriceSection" class="live-price-section" aria-live="polite">
  <div class="live-price-header">
    <span class="live-indicator"></span>
    <span class="live-label">LIVE BTC PRICE</span>
    <button id="refreshPriceBtn" class="refresh-btn" onclick="fetchLivePrice()" aria-label="Refresh BTC price">
      ↻
    </button>
  </div>
  <div class="live-price-value" id="livePrice">Loading...</div>
  <div class="last-updated" id="lastUpdated"></div>
</div>
```

**Visual Design:**
- Background: `#e0e7ff` (light indigo)
- Border: 3px solid `#000`
- Box shadow: 4px 4px 0 `#000`
- Padding: 16px
- Margin bottom: 25px

### 3.2 Price Display Elements

| Element | Styling |
|---------|---------|
| Live indicator dot | 10px circle, `#22c55e` (green), pulsing animation |
| Live label | Uppercase, font-weight 700, font-size 12px, letter-spacing 1px |
| Refresh button | Minimal design, background `#fef08a`, border 2px `#000` |
| Price value | Font-size 28px, font-weight 800, color `#000` |
| Last updated text | Font-size 11px, color `#666`, font-weight 600 |

### 3.3 Comparison Section (In Results)

**Location:** Inside the `.result` div, after the main target price display

**HTML Structure:**
```html
<div class="price-comparison" id="priceComparison">
  <div class="comparison-header">PRICE COMPARISON</div>
  
  <div class="comparison-grid">
    <div class="comparison-item">
      <span class="comparison-label">Current BTC Price:</span>
      <span class="comparison-value" id="comparisonCurrentPrice">$--</span>
    </div>
    
    <div class="comparison-item">
      <span class="comparison-label">Your Target Price:</span>
      <span class="comparison-value" id="comparisonTargetPrice">$--</span>
    </div>
    
    <div class="comparison-item highlight-row">
      <span class="comparison-label">Difference:</span>
      <span class="comparison-value" id="priceDifference">--</span>
    </div>
  </div>
  
  <div class="comparison-indicator" id="comparisonIndicator">
    <!-- Visual indicator inserted via JS -->
  </div>
</div>
```

### 3.4 Comparison Visual States

| State | Condition | Background Color | Border Color | Icon |
|-------|-----------|------------------|--------------|------|
| Target Reachable | Live price ≥ Target price | `#d1fae5` (green-100) | `#000` | ✅ |
| Target Above Current | Live price < Target price | `#fca5a5` (red-100) | `#000` | ⚠️ |

**Difference Calculation:**
```javascript
// Percentage above/below current price
const differencePercent = ((targetPrice - currentPrice) / currentPrice) * 100;

// Display format
// If positive: "+XX.XX% above current price"
// If negative: "XX.XX% below current price"
```

---

## 4. Data Flow

### 4.1 Initial Page Load

```
┌─────────────────┐
│   Page Load     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ fetchLivePrice()│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Call CoinGecko │
│      API        │
└────────┬────────┘
         │
    ┌────┴────┐
    │ Success │
    └────┬────┘
         │
         ▼
┌─────────────────┐
│ Update DOM with │
│ live price &    │
│ timestamp       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Start 60s      │
│ auto-refresh    │
└─────────────────┘
```

### 4.2 Calculate Button Click

```
┌─────────────────┐
│  calculate()    │
│    called       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Calculate      │
│ target price   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Get live price │
│ from state     │
└────────┬────────┘
         │
    ┌────┴────┐
    │ Has     │
    │ price?  │
    └────┬────┘
    Yes  │  No
         │  │
         ▼  ▼
┌────────┐ ┌────────────┐
│ Render │ │ Render      │
│comparison│ results    │
│section │ │ without     │
│        │ │ comparison  │
└────────┘ └────────────┘
```

### 4.3 Auto-Refresh Cycle

```
┌─────────────────┐
│ 60 seconds      │
│ elapsed         │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ fetchLivePrice()│
│ (silent update) │
└────────┬────────┘
         │
    ┌────┴────┐
    │ Success │
    └────┬────┘
         │
         ▼
┌─────────────────┐
│ Update price   │
│ & timestamp     │
│ (no UI flash)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ If results     │
│ visible,       │
│ update comp.   │
└─────────────────┘
```

### 4.4 State Management

```javascript
// Global state object
const appState = {
  livePrice: null,
  lastUpdated: null,
  isLoading: false,
  error: null,
  refreshInterval: null
};
```

---

## 5. Error Handling

### 5.1 API Failure Scenarios

| Scenario | User Display | Action |
|----------|--------------|--------|
| Network timeout | "Price unavailable" | Show cached price if available |
| 429 Rate limit | "Rate limited, retrying in 60s" | Auto-retry after cooldown |
| 5xx Server error | "Price temporarily unavailable" | Show retry button |
| CORS blocked | "Unable to fetch price" | Show manual refresh option |
| No internet | "No internet connection" | Show cached/error state |

### 5.2 Error UI Implementation

```html
<div id="priceError" class="price-error" style="display: none;">
  <span class="error-icon">⚠️</span>
  <span class="error-message" id="priceErrorMessage">Unable to fetch live price</span>
  <button class="retry-btn" onclick="fetchLivePrice()">Retry</button>
</div>
```

**Styling:**
- Background: `#fca5a5` (red-100)
- Border: 3px solid `#000`
- Box shadow: 4px 4px 0 `#000`
- Error icon: ⚠️ emoji or SVG
- Retry button: Background `#fef08a`, border 2px `#000`

### 5.3 Fallback Behavior

- **First load failure:** Show "Price unavailable" with retry button
- **Subsequent failures during refresh:** Keep showing last known price with "Last updated: X ago" indicator
- **Calculation without live price:** Hide comparison section, show results normally

---

## 6. Styling (Neo-Brutalist Aesthetic)

### 6.1 CSS Variables

```css
:root {
  /* Live price section */
  --live-bg: #e0e7ff;
  --live-border: #000;
  --live-shadow: #000;
  
  /* Price states */
  --price-success-bg: #d1fae5;
  --price-warning-bg: #fca5a5;
  
  /* Text */
  --text-primary: #000;
  --text-secondary: #666;
  
  /* Indicator */
  --indicator-live: #22c55e;
}
```

### 6.2 New CSS Classes

```css
/* Live Price Section */
.live-price-section {
  background: var(--live-bg);
  border: 3px solid var(--live-border);
  box-shadow: 4px 4px 0 var(--live-shadow);
  padding: 16px;
  margin-bottom: 25px;
  border-radius: 0;
}

.live-price-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 8px;
}

.live-indicator {
  width: 10px;
  height: 10px;
  background: var(--indicator-live);
  border: 2px solid #000;
  border-radius: 50%;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.live-label {
  font-weight: 700;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.refresh-btn {
  margin-left: auto;
  background: #fef08a;
  border: 2px solid #000;
  padding: 4px 10px;
  cursor: pointer;
  font-weight: 700;
  font-size: 14px;
  box-shadow: 2px 2px 0 #000;
  transition: all 0.15s ease;
}

.refresh-btn:hover {
  transform: translate(1px, 1px);
  box-shadow: 1px 1px 0 #000;
}

.live-price-value {
  font-size: 28px;
  font-weight: 800;
  color: var(--text-primary);
  margin: 8px 0;
}

.last-updated {
  font-size: 11px;
  color: var(--text-secondary);
  font-weight: 600;
}

/* Price Error */
.price-error {
  background: var(--price-warning-bg);
  border: 3px solid #000;
  box-shadow: 4px 4px 0 #000;
  padding: 12px;
  margin-bottom: 25px;
  display: flex;
  align-items: center;
  gap: 8px;
  border-radius: 0;
}

.price-error .retry-btn {
  margin-left: auto;
  background: #fef08a;
  border: 2px solid #000;
  padding: 6px 12px;
  cursor: pointer;
  font-weight: 700;
  font-size: 12px;
  box-shadow: 2px 2px 0 #000;
}

/* Price Comparison */
.price-comparison {
  margin-top: 20px;
  padding-top: 20px;
  border-top: 3px dashed #000;
}

.comparison-header {
  font-weight: 700;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 15px;
  text-align: center;
  background: #a5f3fc;
  padding: 8px;
  border: 2px solid #000;
}

.comparison-grid {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.comparison-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  background: #fff;
  border: 2px solid #000;
}

.comparison-label {
  font-weight: 600;
  font-size: 13px;
}

.comparison-value {
  font-weight: 800;
  font-size: 14px;
}

.highlight-row {
  background: var(--price-success-bg);
}

.highlight-row.warning {
  background: var(--price-warning-bg);
}

.comparison-indicator {
  margin-top: 15px;
  padding: 12px;
  text-align: center;
  font-weight: 700;
  font-size: 14px;
  border: 3px solid #000;
  box-shadow: 4px 4px 0 #000;
}

.comparison-indicator.success {
  background: var(--price-success-bg);
}

.comparison-indicator.warning {
  background: var(--price-warning-bg);
}
```

---

## 7. JavaScript Implementation

### 7.1 Required Functions

| Function | Purpose |
|----------|---------|
| `fetchLivePrice()` | Fetch price from CoinGecko API |
| `updateLivePriceDisplay(price)` | Update DOM with live price |
| `showPriceError(message)` | Display error state |
| `startAutoRefresh()` | Begin 60-second refresh interval |
| `stopAutoRefresh()` | Clear refresh interval |
| `calculate()` (modified) | Add comparison to existing function |
| `formatTimestamp(date)` | Format last updated time |

### 7.2 Modified calculate() Function

The existing `calculate()` function should be modified to:

1. Accept an optional parameter for live price comparison
2. If `appState.livePrice` is available, render the comparison section
3. Calculate the percentage difference between target and live price
4. Apply appropriate CSS classes based on whether target is reachable

---

## 8. Acceptance Criteria

### 8.1 Functional Requirements

- [ ] Live BTC price displays within 2 seconds of page load
- [ ] Price auto-refreshes every 60 seconds without page reload
- [ ] Manual refresh button works correctly
- [ ] Comparison section appears in results when live price is available
- [ ] Percentage difference calculates correctly
- [ ] Visual indicators change based on target reachability
- [ ] Error states display appropriately when API fails
- [ ] Last updated timestamp shows accurate time

### 8.2 Visual Requirements

- [ ] Neo-brutalist styling consistent with existing calculator
- [ ] Live indicator pulses with green color
- [ ] Comparison section uses correct colors (green/red)
- [ ] All new elements have proper borders and shadows
- [ ] Responsive design works on mobile (≤ 480px)
- [ ] Loading spinner appears during fetch
- [ ] Error states are clearly visible

### 8.3 Edge Cases

- [ ] Handle API rate limiting gracefully
- [ ] Handle calculation when live price is unavailable
- [ ] Handle extremely large/small price values
- [ ] Handle rapid manual refresh clicks (debounce)
- [ ] Maintain state during page visibility changes

---

## 9. Implementation Priority

| Step | Task | Description |
|------|------|-------------|
| 1 | Add HTML structure | Insert live price section after `<h1>` |
| 2 | Add CSS styles | Implement neo-brutalist styling |
| 3 | Implement fetch function | Create `fetchLivePrice()` with CoinGecko API |
| 4 | Add auto-refresh | Implement 60-second interval |
| 5 | Add error handling | Implement error states and retry |
| 6 | Modify calculate() | Add comparison section to results |
| 7 | Test and verify | Test all scenarios and edge cases |
