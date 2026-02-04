# Fractal Model Pro (TTrades) - Requirements Document

## 1. Executive Summary

This document outlines the requirements for developing a Pinescript indicator that replicates the **Fractal Model Pro (TTrades)** indicator. The indicator is designed to identify Algorithmic Price Delivery patterns by analyzing price movements across multiple timeframes, detecting momentum shifts, swing formations, and orderflow continuations.

**Key Characteristics:**
- Non-repainting and stable within the given time period
- Multi-timeframe analysis (Higher Timeframe / Lower Timeframe pairing)
- Automated detection of price delivery patterns
- Customizable visualization and alert system

---

## 2. Core Concept & Framework

### 2.1 Fundamental Principle

The Fractal Model is based on the cyclical nature of price movements, where price alternates between large and small ranges. **Expansion** occurs when price moves consistently in one direction with momentum. The model identifies moments when expansion is poised to occur by:

1. Combining **Higher Timeframe (HTF) closures** with
2. **Change in State of Delivery (CISD)** confirmation on the **Lower Timeframe (LTF)**

### 2.2 Timeframe Pairing System

The indicator operates on a **fractal pairing** concept where:
- **Higher Timeframe (HTF)**: Provides the structural context (e.g., 1H, 4H, 1D)
- **Lower Timeframe (LTF)**: Shows detailed price action within HTF candles (e.g., 5m, 15m)

**Supported Pairings:**
- Standard pairings: 5m-1H, 15m-4H, etc.
- Custom pairings: User-defined HTF-LTF combinations
- Automatic pairing: System automatically selects appropriate HTF based on current chart timeframe

**Important Limitation:**
- If viewing a chart timeframe greater than the LTF (e.g., viewing 15m chart when 5m-1H is enabled), the indicator displays a warning. HTF candle plotting remains visible, but LTF CISD and projections will not render.

---

## 3. Core Components & Mechanics

### 3.1 Higher Timeframe (HTF) Candles

**Purpose:** Visualize the structural context of price action on a higher timeframe.

**Mechanics:**
- Plot HTF candles on the LTF chart
- Each HTF candle represents the complete price action for that timeframe period
- HTF candles use Power of Three (PO3) framework to identify critical turning points

**Customization Requirements:**
- **Candle Size**: Adjustable size for visibility
- **Offset**: Positive/negative offset to shift candles left/right
- **Visibility Toggle**: Option to hide/show HTF candles
- **Colors**: Customizable body, border, and wick colors
- **Markers**: Optional open/close markers and high/low lines
- **Lookback History**: Control how many HTF candles are plotted (NEW feature)

**Previous Candle EQ:**
- Display equilibrium (50% level) of the previous HTF candle
- Can be viewed on both HTF candle and chart directly

### 3.2 Change in State of Delivery (CISD)

**Purpose:** Identify the first potential change in orderflow, signaling a shift from sell program to buy program or vice versa.

**Mechanics:**
- Marks the series of candles making up significant highs or lows
- **CISD Confirmation:** A close beyond the opening price signals a change from bullish to bearish (or vice versa)
- This confirms a trend reversal and is a form of orderblock
- **Early CISD (NEW):** Option to highlight early CISD within the C2 candle for earlier detection

**Visual Requirements:**
- Customizable line style and width
- Clear marking of CISD candles on the chart

### 3.3 TTrades Framework Labels (C2/C3/C4)

**Purpose:** Identify key structural points in the fractal formation.

**Label States & Logic:**

1. **Gray Label (Valid Setup):**
   - Setup remains valid
   - All projections, Equilibrium, Liquidity Sweep, and T-spot are plotted
   - Indicates stable conditions

2. **Red Label (Failed Setup):**
   - Setup fails when price returns to the initial high or low without forming a higher timeframe swing point
   - Indicator stops plotting: projections, Equilibrium, Liquidity Sweep, and T-spot
   - Labels (C2, C3, C4) remain on chart but turn red
   - Clearly indicates setup failure

3. **Orange Label (Consolidation):**
   - If setup does not fail within the next higher timeframe candle (which defines the setup's formation)
   - Label turns orange
   - Signals potential consolidation or slowdown
   - Suggests market may enter a range or pause in trend movement

**Customization:**
- Toggle to show/hide labels
- Adjustable label size

### 3.4 Candle 1 Liquidity Sweep

**Purpose:** Highlight important liquidity levels at each swing point.

**Mechanics:**
- Horizontal rays mark sweeps of liquidity
- Identifies potential reversal points
- Marks the initial liquidity marker in the formation

**Customization:**
- Line type (solid, dotted, dashed)
- Line width
- Color options

### 3.5 Candle Equilibrium (EQ)

**Purpose:** Indicate 50% levels of higher timeframe ranges, displaying discount and premium zones.

**Mechanics:**
- Calculates 50% midpoint of HTF candle range
- **Premium Zone:** Upper 50% of range (best short setups form here)
- **Discount Zone:** Lower 50% of range (best long setups form here)
- Provides context for potential entries and exits

**Conceptual Rule:**
- Best short setups form inside premium end of range
- Best long setups form inside discount end of range
- "Buy when it's cheap, sell when it's expensive"

**Customization:**
- Toggle to show/hide equilibrium lines
- Line style and color options

### 3.6 T-Spot Identification

**Purpose:** Mark anticipated points where HTF candle wicks are expected to form.

**Mechanics:**
- Based on TTrades' refined analysis and methodology
- Projects potential area where wick of new HTF candle will form
- Identifies high-probability reversal or continuation points within lower timeframes
- Remains aligned with higher timeframe narrative

**Customization:**
- Toggle to enable/disable T-Spot
- Marker style and appearance options

### 3.7 Projections

**Purpose:** Leverage projected levels based on shifts in delivery for future price redelivery, rebalancing, and exhaustion points.

**Mechanics:**
- User-defined projection levels based on past orderblock behavior
- Default projections: [-1, -2, -2.5, -4, -4.5]
- Two types of projections:
  - **Body Projections:** Based on HTF candle body
  - **Wick Projections:** Based on HTF candle wicks

**Calculation Logic:**
- Projections extend from key reference points (likely C2, C3, C4 or CISD points)
- Multipliers determine distance from reference (e.g., -1 = 1x range, -2 = 2x range)
- Negative values suggest extension in opposite direction of trend

**Customization:**
- Add or remove projection levels
- Enable/disable projection display
- Label visibility and styling
- Label size, style, and color customization

### 3.8 Formation Liquidity

**Purpose:** Identify previous candles' highs and lows as critical liquidity points for the current developing formation.

**Mechanics:**
- Marks engineered liquidity pools
- Highlights previous swing highs and lows
- Serves as key reference points for future price action
- Helps identify areas where price may seek liquidity

**Customization:**
- Toggle to show/hide liquidity markers
- Line style (dotted/solid)
- Line width
- Color options

---

## 4. Setup Detection & Validation Logic

### 4.1 Setup Formation Process

**Step-by-Step Logic:**

1. **HTF Candle Closure:**
   - Wait for HTF candle to close
   - This establishes the structural context

2. **CISD Detection on LTF:**
   - Monitor LTF price action within the HTF candle
   - Detect when a close occurs beyond the opening price (CISD confirmation)
   - This marks the first potential change in orderflow

3. **Label Assignment (C2/C3/C4):**
   - C2: First significant point after CISD
   - C3: Subsequent structural point
   - C4: Final structural point in formation
   - Labels help track the formation progression

4. **Setup Validation:**
   - Setup remains valid (gray) if price continues in expected direction
   - Setup fails (red) if price returns to initial high/low without forming HTF swing point
   - Setup consolidates (orange) if no failure but no progression within next HTF candle

5. **Projection Calculation:**
   - Once setup is validated, calculate projections from key points
   - Plot equilibrium, T-spot, and liquidity levels

### 4.2 Setup Failure Conditions

**Failure Criteria:**
- Price returns to the initial high (for bullish setups) or initial low (for bearish setups)
- This return occurs WITHOUT forming a higher timeframe swing point
- When failure occurs:
  - Stop plotting: projections, EQ, Liquidity Sweep, T-spot
  - Change labels to red
  - Keep labels visible for reference

### 4.3 Consolidation Detection

**Consolidation Criteria:**
- Setup does not fail within the next higher timeframe candle
- Setup does not progress as expected
- Market enters range-bound or paused state
- Label turns orange to signal potential slowdown

---

## 5. Bias Selection & Filtering

### 5.1 Bias Options

**Three Bias Modes:**

1. **Bullish:** Only detect and display bullish formations
2. **Bearish:** Only detect and display bearish formations
3. **Neutral:** Monitor and display both bullish and bearish formations

### 5.2 Auto Bias Feature (NEW)

**Auto Bias 1:**
- Aligns fractal models with one timeframe higher than current chart
- Automatic bias selection based on HTF context

**Auto Bias 2:**
- Aligns fractal models with two timeframes higher than current chart
- Provides even higher timeframe context

**Manual Bias:**
- User can manually set bias independent of timeframe

---

## 6. Time Filtering System

### 6.1 Time Filter Threshold

**Purpose:** Specify a timeframe threshold below which time filters will be applied.

**Logic:**
- If current chart timeframe is below threshold, apply time filters
- If current chart timeframe is above threshold, ignore time filters
- Helps focus on relevant trading sessions

### 6.2 Custom Time Filters

**Mechanics:**
- Define up to **three custom time windows** (e.g., 02:00 - 05:00, 08:00 - 11:00)
- Filter model formations that fall outside these specified ranges
- Provides clarity on relevant price action signatures within defined time windows

**Custom Timezone:**
- Define custom timezone based on UTC
- Default: New York Eastern Time
- Allows alignment with specific trading sessions

---

## 7. SMT Divergence Module (NEW Feature)

### 7.1 Purpose

Detect divergences between correlated or inversely correlated markets directly within the Fractal Model.

### 7.2 Mechanics

**SMT Pair Selection:**
- **Manual:** User selects two markets to compare (e.g., CME_MINI:ES1! and CME_MINI:YM1!)
- **Automatic:** System automatically selects most relevant SMT pair for charted asset
- **Inverse Correlation:** Option to mark if selected market has inverse correlation
  - Note: If Auto SMT and Inverse are both enabled, inverse setting is ignored

**Divergence Detection:**
- Compare price action between two markets
- Identify when markets diverge from expected correlation
- Signal potential reversal or continuation opportunities

### 7.3 Customization

- Toggle to show/hide SMT lines
- Enable alerts for divergence detection
- Line style, width, and color
- Label display (size: Tiny → Huge)

---

## 8. Information Display

### 8.1 Info Table

**Purpose:** Display key information about current fractal model state.

**Information Displayed:**
- Fractal pairing (HTF-LTF)
- Time until next HTF candle close
- Current analyst bias
- Applied time filter preferences

**Customization:**
- Table or on-chart display options
- Position (top-left, top-right, bottom-left, bottom-right, etc.)
- Size adjustment
- Border toggle for table cells
- Select which information details to display

---

## 9. History & Lookback Settings

### 9.1 Custom History

**Purpose:** Control depth of historical view.

**Options:**
- Current setup only (0)
- Up to 40 previous setups
- Allows analysis of past formations for context

**Use Cases:**
- View context of previous setups
- Collect data on model performance
- Identify patterns across multiple setups

### 9.2 HTF Candle Lookback History (NEW)

**Purpose:** Define how many HTF candles are plotted on chart.

**Logic:**
- Increasing value: More historical structure, broader view of swing development
- Lower values: Focus on most recent expansion
- Balances historical context with chart clarity

---

## 10. Alert System

### 10.1 Alert Setup Process

**Steps:**
1. Toggle 'Alerts' ON in indicator settings
2. Select preferred alert conditions
3. Right-click indicator or click three dots next to indicator title
4. Select 'TTFM [Pro+]' or indicator name
5. Choose symbol or pre-defined watchlist
6. Set alert name (optional)
7. Click 'Create'

### 10.2 Alert Conditions

**Available Alerts:**
- Setup formation detected
- Setup failure
- Setup consolidation
- CISD confirmation
- T-Spot reached
- Projection level reached
- SMT divergence detected (if enabled)

**Alert Requirements:**
- Alerts must be enabled in settings
- Individual alert conditions must be selected
- Platform alert system must be configured

---

## 11. Visual Customization Requirements

### 11.1 General Styling

All visual elements must support:
- Color customization
- Line style options (solid, dotted, dashed)
- Line width adjustment
- Visibility toggles
- Size adjustments (where applicable)

### 11.2 Specific Visual Elements

**HTF Candles:**
- Body, border, wick colors
- Size adjustment
- Offset positioning
- Open/close markers
- High/low lines

**CISD Lines:**
- Line style and width
- Color options

**Equilibrium Lines:**
- Line style and color
- Previous candle EQ display option

**T-Spot:**
- Marker style
- Color and size

**Projections:**
- Line style and color
- Label visibility
- Label size, style, color

**Liquidity Markers:**
- Line style (dotted/solid)
- Line width
- Color

**Labels (C2/C3/C4):**
- Size adjustment
- Color states (gray/red/orange)
- Visibility toggle

---

## 12. Technical Requirements

### 12.1 Non-Repainting Behavior

**Critical Requirement:**
- Indicator must be **non-repainting** and **stable**
- Once a setup is formed within a time period, it must not change
- Levels and labels must remain unchanged within the given time period
- Only new time periods should generate new setups

**Implementation Note:**
- Use `request.security()` for HTF data
- Ensure calculations are based on confirmed (closed) candles only
- Avoid forward-looking functions that could cause repainting

### 12.2 Multi-Timeframe Handling

**Requirements:**
- Properly handle HTF-LTF relationships
- Detect when chart timeframe is incompatible with selected pairing
- Display warning message when LTF analysis cannot be performed
- Maintain HTF candle plotting even when LTF is unavailable

### 12.3 Performance Considerations

- Efficient calculation of multiple setups (up to 40 history)
- Optimized rendering of HTF candles and projections
- Smooth operation across different timeframe pairings
- Minimal impact on chart performance

---

## 13. Calculation Logic Details

### 13.1 HTF Candle Calculation

**Process:**
1. Request HTF data using `request.security()`
2. Identify HTF candle open, high, low, close
3. Calculate HTF candle range
4. Determine if current LTF candle is within HTF candle period
5. Plot HTF candle with appropriate offset

### 13.2 CISD Detection Logic

**For Bullish CISD:**
- Identify series of candles making up significant low
- Detect when close occurs above opening price
- Mark this as change from bearish to bullish delivery

**For Bearish CISD:**
- Identify series of candles making up significant high
- Detect when close occurs below opening price
- Mark this as change from bullish to bearish delivery

### 13.3 Equilibrium Calculation

**Formula:**
```
EQ = (HTF_High + HTF_Low) / 2
```

**Previous Candle EQ:**
```
Previous_EQ = (Previous_HTF_High + Previous_HTF_Low) / 2
```

### 13.4 Projection Calculation

**Body Projections:**
- Reference point: Typically C2, C3, or CISD point
- Range: HTF candle body size or range
- Calculation: `Projection_Level = Reference_Point ± (Range × Multiplier)`

**Wick Projections:**
- Reference point: HTF candle high or low (wick extremes)
- Range: HTF candle range
- Calculation: `Projection_Level = Reference_Point ± (Range × Multiplier)`

**Default Multipliers:** -1, -2, -2.5, -4, -4.5

### 13.5 T-Spot Calculation

**Logic:**
- Based on TTrades' methodology (specific algorithm to be determined from PDF analysis)
- Projects anticipated wick formation area
- Typically relates to previous structure and current momentum
- Must align with HTF narrative

**Note:** Exact calculation method may require additional research or reverse engineering from existing implementation.

---

## 14. Edge Cases & Error Handling

### 14.1 Invalid Timeframe Pairings

- Display warning when chart timeframe > LTF
- Continue showing HTF candles but disable LTF features
- Clear user notification about limitation

### 14.2 Insufficient Data

- Handle cases where not enough historical data exists
- Gracefully degrade functionality (e.g., fewer historical setups)
- Avoid errors when data is unavailable

### 14.3 Market Hours & Gaps

- Handle market gaps appropriately
- Account for different market sessions
- Time filter application during non-trading hours

### 14.4 SMT Pair Unavailability

- Handle cases where SMT pair data is unavailable
- Disable SMT features gracefully
- Notify user if automatic pair detection fails

---

## 15. Testing Requirements

### 15.1 Functional Testing

- Verify non-repainting behavior across multiple timeframes
- Test all bias modes (bullish, bearish, neutral)
- Validate setup detection and failure logic
- Test all customization options
- Verify alert functionality

### 15.2 Timeframe Testing

- Test all standard timeframe pairings
- Test custom timeframe pairings
- Test automatic pairing feature
- Verify behavior when chart timeframe > LTF
- Test across different assets (stocks, forex, crypto)

### 15.3 Visual Testing

- Verify all visual elements render correctly
- Test color customization
- Verify label states (gray/red/orange)
- Test table positioning and display
- Verify HTF candle plotting accuracy

---

## 16. Deliverables

### 16.1 Pinescript Indicator

- Complete, functional Pinescript indicator
- All features as specified in this document
- Properly commented code
- Error handling and edge case management

### 16.2 Documentation

- User guide for indicator settings
- Explanation of each feature
- Recommended settings for different trading styles
- Troubleshooting guide

### 16.3 Testing Report

- Verification of all features
- Performance metrics
- Known limitations or issues

---

## 17. Acceptance Criteria

The indicator will be considered complete when:

1. ✅ All core components function correctly (HTF candles, CISD, labels, projections, etc.)
2. ✅ Non-repainting behavior is verified and stable
3. ✅ All customization options work as specified
4. ✅ Alert system functions properly
5. ✅ Time filtering and bias selection work correctly
6. ✅ SMT divergence module functions (if implemented)
7. ✅ Visual elements render correctly and are customizable
8. ✅ Info table displays accurate information
9. ✅ History/lookback features work as specified
10. ✅ Indicator performs well across different timeframes and assets
11. ✅ All edge cases are handled gracefully
12. ✅ Code is clean, commented, and maintainable

---

## 18. Notes & Assumptions

### 18.1 Methodology Assumptions

- **T-Spot Calculation:** Exact algorithm may need refinement based on TTrades' methodology. May require iterative testing and adjustment.
- **C2/C3/C4 Assignment:** Specific logic for assigning these labels may need clarification through testing or additional documentation.
- **Projection Reference Points:** Exact reference points for projections (C2, C3, C4, or CISD) may need determination through analysis.

### 18.2 Implementation Notes

- Pinescript limitations may require creative solutions for some features
- `request.security()` will be essential for HTF data
- Consider using arrays for managing multiple historical setups
- Label management may require careful state tracking

### 18.3 Future Enhancements

- Additional projection levels
- More SMT pair options
- Enhanced visualization options
- Performance optimizations

---

## 19. References

- **Source Documentation:** [Toodegrees Fractal Model Pro Documentation](https://helpcenter.toodegrees.trade/articles/6644257-fractal-model-pro-ttrades)
- **PDF Documentation:** Files in Documentation folder
- **Framework:** TTrades Fractal Model methodology
- **Related Concepts:** ICT (Inner Circle Trader) methodologies, Power of Three framework

---

## Document Version

**Version:** 1.0  
**Date:** [Current Date]  
**Status:** Draft for Review

---

## Sign-Off

**Customer Approval:**

Name: _________________________  
Signature: _____________________  
Date: __________________________

**Developer Acknowledgment:**

Name: _________________________  
Signature: _____________________  
Date: __________________________

---

*This requirements document serves as the official specification for the Fractal Model Pro (TTrades) Pinescript indicator development project. Any changes or modifications must be agreed upon by both parties and documented as amendments to this document.*
