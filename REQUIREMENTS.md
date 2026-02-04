# Fractal Model Pro (TTrades) - Requirements Document

## 1. Executive Summary

This document outlines the requirements for developing a Pinescript indicator that replicates the **Fractal Model Pro (TTrades)** indicator. The indicator is designed to identify Algorithmic Price Delivery patterns by analyzing price movements across multiple timeframes, detecting momentum shifts, swing formations, and orderflow continuations.

**Key Characteristics:**
- Non-repainting and stable within the given time period
- Multi-timeframe analysis (Higher Timeframe / Lower Timeframe pairing)
- Automated detection of price delivery patterns
- Customizable visualization and alert system

---

## 2. Inputs & Parameters

This section details all user-configurable inputs/parameters for the indicator. Each input is explained with its purpose, valid range, default value, and impact on indicator behavior.

### 2.1 General Settings

#### 2.1.1 Alerts Toggle
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Master switch to enable/disable all alert functionality
- **Behavior:** When OFF, no alerts will trigger regardless of individual alert settings. When ON, individual alert conditions can be enabled/disabled separately.
- **Impact:** Does not affect indicator calculations, only alert triggering.

#### 2.1.2 History Setups
- **Type:** Integer (slider or input)
- **Range:** 0 to 40
- **Default:** 0 (current setup only)
- **Purpose:** Controls how many past setups are displayed on the chart
- **Behavior:** 
  - 0 = Only current/active setup visible
  - 1-40 = Display that many historical setups
- **Impact:** 
  - Higher values provide more historical context but may clutter chart
  - Lower values keep focus on current setup
  - Affects performance: more setups = more calculations and rendering
- **Use Case:** 
  - 0: Clean chart, focus on current opportunity
  - 5-10: Moderate historical context
  - 20-40: Extensive historical analysis, pattern recognition

#### 2.1.3 Fractal Pairing Mode
- **Type:** Enum (dropdown)
- **Options:** 
  - "Automatic" - System selects HTF automatically
  - "Custom" - User defines HTF manually
- **Default:** "Automatic"
- **Purpose:** Determines how the Higher Timeframe is selected
- **Behavior:**
  - **Automatic:** Uses predefined rules based on current chart timeframe
    - 1m chart → 5m HTF
    - 5m chart → 15m or 1H HTF
    - 15m chart → 1H or 4H HTF
    - 1H chart → 4H or 1D HTF
    - 4H chart → 1D HTF
    - 1D chart → 1W HTF
  - **Custom:** User must specify HTF timeframe in separate input
- **Impact:** Affects all HTF-dependent calculations and visualizations

#### 2.1.4 Custom Higher Timeframe
- **Type:** Timeframe (dropdown)
- **Options:** All TradingView supported timeframes
- **Default:** "1H" (when Custom mode selected)
- **Purpose:** User-defined HTF when Fractal Pairing Mode is set to "Custom"
- **Behavior:** Only active when Fractal Pairing Mode = "Custom"
- **Validation:** Must be greater than current chart timeframe, otherwise warning displayed
- **Impact:** Determines structural context for all fractal model calculations

#### 2.1.5 Bias Selection
- **Type:** Enum (dropdown)
- **Options:**
  - "Neutral" - Show both bullish and bearish setups
  - "Bullish" - Only show bullish setups
  - "Bearish" - Only show bearish setups
  - "Auto Bias 1" - Align with one timeframe higher
  - "Auto Bias 2" - Align with two timeframes higher
- **Default:** "Neutral"
- **Purpose:** Filters which setups are detected and displayed
- **Behavior:**
  - **Neutral:** All valid setups shown regardless of direction
  - **Bullish:** Only setups indicating upward expansion shown
  - **Bearish:** Only setups indicating downward expansion shown
  - **Auto Bias 1:** Bias determined by HTF+1 timeframe trend
  - **Auto Bias 2:** Bias determined by HTF+2 timeframe trend
- **Impact:** 
  - Filters setup detection logic
  - Reduces chart clutter when focusing on one direction
  - Auto modes adapt to higher timeframe context

#### 2.1.6 Time Filter Threshold
- **Type:** Timeframe (dropdown)
- **Options:** All TradingView supported timeframes
- **Default:** "1H"
- **Purpose:** Specifies the maximum chart timeframe where time filters will be applied
- **Behavior:**
  - If current chart timeframe ≤ threshold: Time filters are active
  - If current chart timeframe > threshold: Time filters are ignored
- **Logic:** Time filters are most useful on lower timeframes. On higher timeframes, they become less relevant.
- **Impact:** Determines whether time filter windows are enforced

#### 2.1.7 Time Filter 1 Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable first custom time filter window
- **Behavior:** When enabled, setups are only shown if they occur within Time Filter 1 window
- **Impact:** Filters setup detection based on time of day

#### 2.1.8 Time Filter 1 Start
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "02:00"
- **Purpose:** Start time for first time filter window
- **Behavior:** Uses timezone specified in Custom Timezone setting
- **Impact:** Defines beginning of first allowed time window

#### 2.1.9 Time Filter 1 End
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "05:00"
- **Purpose:** End time for first time filter window
- **Behavior:** Uses timezone specified in Custom Timezone setting. Can wrap around midnight (e.g., 22:00 to 02:00).
- **Impact:** Defines end of first allowed time window

#### 2.1.10 Time Filter 2 Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable second custom time filter window
- **Behavior:** When enabled, setups are only shown if they occur within Time Filter 2 window (or Time Filter 1 if both enabled)
- **Impact:** Allows multiple trading session windows

#### 2.1.11 Time Filter 2 Start
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "08:00"
- **Purpose:** Start time for second time filter window
- **Behavior:** Uses timezone specified in Custom Timezone setting
- **Impact:** Defines beginning of second allowed time window

#### 2.1.12 Time Filter 2 End
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "11:00"
- **Purpose:** End time for second time filter window
- **Behavior:** Uses timezone specified in Custom Timezone setting
- **Impact:** Defines end of second allowed time window

#### 2.1.13 Time Filter 3 Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable third custom time filter window
- **Behavior:** When enabled, setups are only shown if they occur within Time Filter 3 window (or Time Filter 1/2 if multiple enabled)
- **Impact:** Allows up to three distinct trading session windows

#### 2.1.14 Time Filter 3 Start
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "14:00"
- **Purpose:** Start time for third time filter window
- **Behavior:** Uses timezone specified in Custom Timezone setting
- **Impact:** Defines beginning of third allowed time window

#### 2.1.15 Time Filter 3 End
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "17:00"
- **Purpose:** End time for third time filter window
- **Behavior:** Uses timezone specified in Custom Timezone setting
- **Impact:** Defines end of third allowed time window

#### 2.1.16 Custom Timezone
- **Type:** Integer (UTC offset in hours)
- **Range:** -12 to +14
- **Default:** -5 (New York Eastern Time, UTC-5)
- **Purpose:** Defines timezone for time filter calculations
- **Behavior:** 
  - Positive values = UTC+X (e.g., +2 for Central European Time)
  - Negative values = UTC-X (e.g., -5 for Eastern Time)
  - Accounts for daylight saving time if applicable
- **Impact:** All time filter windows use this timezone for start/end times

### 2.2 HTF Candles Settings

#### 2.2.1 HTF Candle Visibility
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide higher timeframe candles on chart
- **Behavior:** When OFF, HTF candles are not plotted but calculations still occur
- **Impact:** Affects visual display only, not calculations

#### 2.2.2 HTF Candle Size
- **Type:** Integer (slider)
- **Range:** 1 to 10
- **Default:** 5
- **Purpose:** Controls the width/thickness of HTF candles
- **Behavior:** Higher values = thicker candles, easier to see but may overlap
- **Impact:** Visual only, affects readability

#### 2.2.3 HTF Candle Offset
- **Type:** Integer (input)
- **Range:** -50 to +50 (bars)
- **Default:** 0
- **Purpose:** Shifts HTF candles left (negative) or right (positive) relative to price bars
- **Behavior:**
  - Positive values: Shift candles to the right (future direction)
  - Negative values: Shift candles to the left (past direction)
- **Impact:** Helps align HTF structure with LTF price action for better visualization

#### 2.2.4 HTF Candle Lookback History
- **Type:** Integer (slider or input)
- **Range:** 1 to 100
- **Default:** 20
- **Purpose:** Controls how many HTF candles are plotted on chart
- **Behavior:** 
  - Lower values: Focus on recent structure
  - Higher values: More historical context
- **Impact:** 
  - Affects chart clarity vs. historical depth
  - Higher values may impact performance

#### 2.2.5 HTF Candle Body Color (Bullish)
- **Type:** Color picker
- **Default:** Green/Blue
- **Purpose:** Color for HTF candle body when bullish (close > open)
- **Impact:** Visual customization

#### 2.2.6 HTF Candle Body Color (Bearish)
- **Type:** Color picker
- **Default:** Red
- **Purpose:** Color for HTF candle body when bearish (close < open)
- **Impact:** Visual customization

#### 2.2.7 HTF Candle Border Color
- **Type:** Color picker
- **Default:** Same as body or contrasting color
- **Purpose:** Border/outline color for HTF candles
- **Impact:** Visual customization, helps distinguish HTF from LTF candles

#### 2.2.8 HTF Candle Wick Color
- **Type:** Color picker
- **Default:** Same as body or gray
- **Purpose:** Color for HTF candle wicks (high/low lines)
- **Impact:** Visual customization

#### 2.2.9 Show HTF Open/Close Markers
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Display markers at HTF candle open and close prices
- **Behavior:** Small markers or lines indicating exact open/close levels
- **Impact:** Helps identify exact HTF candle boundaries

#### 2.2.10 HTF Open/Close Marker Style
- **Type:** Enum (dropdown)
- **Options:** "Circle", "Square", "Diamond", "Cross", "Line"
- **Default:** "Circle"
- **Purpose:** Visual style for open/close markers
- **Impact:** Visual customization

#### 2.2.11 HTF Open/Close Marker Size
- **Type:** Integer (slider)
- **Range:** 1 to 10
- **Default:** 3
- **Purpose:** Size of open/close markers
- **Impact:** Visibility and chart clutter

#### 2.2.12 Show HTF High/Low Lines
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Display horizontal lines at HTF candle high and low
- **Behavior:** Extends lines across the HTF candle period
- **Impact:** Clearly marks HTF range boundaries

#### 2.2.13 HTF High/Low Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for high/low lines
- **Impact:** Visual customization

#### 2.2.14 HTF High/Low Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of high/low lines
- **Impact:** Visibility

#### 2.2.15 Show Previous Candle EQ
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display equilibrium (50% level) of previous HTF candle
- **Behavior:** Shows horizontal line at previous HTF candle midpoint
- **Impact:** Provides reference for premium/discount zones

#### 2.2.16 Previous Candle EQ Color
- **Type:** Color picker
- **Default:** Gray or light blue
- **Purpose:** Color for previous candle equilibrium line
- **Impact:** Visual customization

#### 2.2.17 Previous Candle EQ Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for previous candle EQ
- **Impact:** Visual customization

### 2.3 Model Style Settings

#### 2.3.1 Show TTrades Fractal Model Labels
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display C2, C3, C4 labels on chart
- **Behavior:** When OFF, labels are hidden but calculations continue
- **Impact:** Visual only, affects chart clarity

#### 2.3.2 Label Size
- **Type:** Enum (dropdown)
- **Options:** "Tiny", "Small", "Normal", "Large", "Huge"
- **Default:** "Normal"
- **Purpose:** Size of C2/C3/C4 labels
- **Impact:** Visibility and chart clutter

#### 2.3.3 Show Candle 1 Sweep
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display liquidity sweep markers at Candle 1 (initial swing point)
- **Behavior:** Horizontal rays marking liquidity levels
- **Impact:** Visual identification of liquidity zones

#### 2.3.4 Candle 1 Sweep Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"
- **Purpose:** Line style for Candle 1 sweep markers
- **Impact:** Visual customization

#### 2.3.5 Candle 1 Sweep Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 2
- **Purpose:** Thickness of Candle 1 sweep lines
- **Impact:** Visibility

#### 2.3.6 Candle 1 Sweep Color
- **Type:** Color picker
- **Default:** Yellow or orange
- **Purpose:** Color for Candle 1 sweep markers
- **Impact:** Visual customization

#### 2.3.7 Show CISD (Change in State of Delivery)
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display CISD lines marking orderflow changes
- **Behavior:** Highlights candles where delivery state changes
- **Impact:** Critical for setup identification

#### 2.3.8 CISD Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"
- **Purpose:** Line style for CISD markers
- **Impact:** Visual customization

#### 2.3.9 CISD Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 2
- **Purpose:** Thickness of CISD lines
- **Impact:** Visibility

#### 2.3.10 CISD Color (Bullish)
- **Type:** Color picker
- **Default:** Green or blue
- **Purpose:** Color for bullish CISD (bearish to bullish change)
- **Impact:** Visual identification of direction

#### 2.3.11 CISD Color (Bearish)
- **Type:** Color picker
- **Default:** Red
- **Purpose:** Color for bearish CISD (bullish to bearish change)
- **Impact:** Visual identification of direction

#### 2.3.12 Show Early CISD
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Highlight early CISD within C2 candle for earlier detection
- **Behavior:** Shows potential CISD before full confirmation
- **Impact:** Earlier setup identification, may have lower reliability

#### 2.3.13 Early CISD Color
- **Type:** Color picker
- **Default:** Light blue or cyan
- **Purpose:** Color for early CISD markers (distinct from confirmed CISD)
- **Impact:** Visual distinction between early and confirmed CISD

#### 2.3.14 Show Candle Equilibrium
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display 50% equilibrium level of HTF candles
- **Behavior:** Horizontal line at midpoint of HTF candle range
- **Impact:** Identifies premium/discount zones

#### 2.3.15 Equilibrium Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for equilibrium lines
- **Impact:** Visual customization

#### 2.3.16 Equilibrium Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of equilibrium lines
- **Impact:** Visibility

#### 2.3.17 Equilibrium Color
- **Type:** Color picker
- **Default:** Gray or white
- **Purpose:** Color for equilibrium lines
- **Impact:** Visual customization

#### 2.3.18 Show T-Spot
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display T-Spot markers indicating anticipated wick formation areas
- **Behavior:** Marks projected areas where HTF candle wicks may form
- **Impact:** Identifies high-probability reversal/continuation zones

#### 2.3.19 T-Spot Marker Style
- **Type:** Enum (dropdown)
- **Options:** "Circle", "Square", "Diamond", "Cross", "Arrow"
- **Default:** "Circle"
- **Purpose:** Visual style for T-Spot markers
- **Impact:** Visual customization

#### 2.3.20 T-Spot Marker Size
- **Type:** Integer (slider)
- **Range:** 1 to 10
- **Default:** 5
- **Purpose:** Size of T-Spot markers
- **Impact:** Visibility

#### 2.3.21 T-Spot Color
- **Type:** Color picker
- **Default:** Purple or magenta
- **Purpose:** Color for T-Spot markers
- **Impact:** Visual customization

### 2.4 Projections Settings

#### 2.4.1 Show Projections
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Master switch for all projection levels
- **Behavior:** When OFF, no projections displayed regardless of individual settings
- **Impact:** Visual only

#### 2.4.2 Projection Type
- **Type:** Enum (dropdown)
- **Options:** "Body Projections", "Wick Projections", "Both"
- **Default:** "Both"
- **Purpose:** Determines which type of projections to calculate and display
- **Behavior:**
  - **Body Projections:** Based on HTF candle body size
  - **Wick Projections:** Based on HTF candle wick extremes
  - **Both:** Display both types
- **Impact:** Affects calculation method and visual display

#### 2.4.3 Projection Level 1 Multiplier
- **Type:** Float (input)
- **Range:** -10.0 to +10.0
- **Default:** -1.0
- **Purpose:** First projection level multiplier
- **Behavior:** Multiplies reference range to calculate projection distance
- **Impact:** Defines first target level

#### 2.4.4 Projection Level 1 Enable
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide first projection level
- **Impact:** Visual only

#### 2.4.5 Projection Level 2 Multiplier
- **Type:** Float (input)
- **Range:** -10.0 to +10.0
- **Default:** -2.0
- **Purpose:** Second projection level multiplier
- **Impact:** Defines second target level

#### 2.4.6 Projection Level 2 Enable
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide second projection level
- **Impact:** Visual only

#### 2.4.7 Projection Level 3 Multiplier
- **Type:** Float (input)
- **Range:** -10.0 to +10.0
- **Default:** -2.5
- **Purpose:** Third projection level multiplier
- **Impact:** Defines third target level

#### 2.4.8 Projection Level 3 Enable
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide third projection level
- **Impact:** Visual only

#### 2.4.9 Projection Level 4 Multiplier
- **Type:** Float (input)
- **Range:** -10.0 to +10.0
- **Default:** -4.0
- **Purpose:** Fourth projection level multiplier
- **Impact:** Defines fourth target level

#### 2.4.10 Projection Level 4 Enable
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide fourth projection level
- **Impact:** Visual only

#### 2.4.11 Projection Level 5 Multiplier
- **Type:** Float (input)
- **Range:** -10.0 to +10.0
- **Default:** -4.5
- **Purpose:** Fifth projection level multiplier
- **Impact:** Defines fifth target level

#### 2.4.12 Projection Level 5 Enable
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide fifth projection level
- **Impact:** Visual only

#### 2.4.13 Show Projection Labels
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display text labels on projection lines showing multiplier values
- **Impact:** Helps identify which projection is which

#### 2.4.14 Projection Label Size
- **Type:** Enum (dropdown)
- **Options:** "Tiny", "Small", "Normal", "Large"
- **Default:** "Small"
- **Purpose:** Size of projection labels
- **Impact:** Visibility and chart clutter

#### 2.4.15 Projection Label Color
- **Type:** Color picker
- **Default:** White or gray
- **Purpose:** Color for projection labels
- **Impact:** Visual customization

#### 2.4.16 Projection Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for all projection lines
- **Impact:** Visual customization

#### 2.4.17 Projection Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of projection lines
- **Impact:** Visibility

#### 2.4.18 Projection Line Color
- **Type:** Color picker
- **Default:** Blue or cyan
- **Purpose:** Color for projection lines
- **Impact:** Visual customization

### 2.5 Formation Liquidity Settings

#### 2.5.1 Show Formation Liquidity
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display liquidity markers from previous candles
- **Behavior:** Marks previous swing highs and lows as liquidity zones
- **Impact:** Identifies engineered liquidity pools

#### 2.5.2 Formation Liquidity Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for liquidity markers
- **Impact:** Visual customization

#### 2.5.3 Formation Liquidity Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of liquidity marker lines
- **Impact:** Visibility

#### 2.5.4 Formation Liquidity Color
- **Type:** Color picker
- **Default:** Yellow or light orange
- **Purpose:** Color for liquidity markers
- **Impact:** Visual customization

### 2.6 SMT Divergence Settings

#### 2.6.1 Show SMT Divergence
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable SMT (Smart Money Tool) divergence detection
- **Behavior:** Compares price action between correlated markets
- **Impact:** Adds divergence analysis to fractal model

#### 2.6.2 SMT Mode
- **Type:** Enum (dropdown)
- **Options:** "Automatic", "Manual"
- **Default:** "Automatic"
- **Purpose:** How SMT pair is selected
- **Behavior:**
  - **Automatic:** System selects most relevant pair for current asset
  - **Manual:** User specifies both markets
- **Impact:** Determines which markets are compared

#### 2.6.3 SMT Pair 1 (Manual Mode)
- **Type:** String (symbol input)
- **Default:** ""
- **Purpose:** First market symbol for manual SMT pair
- **Example:** "CME_MINI:ES1!" (E-mini S&P 500)
- **Validation:** Must be valid TradingView symbol
- **Impact:** Only used when SMT Mode = "Manual"

#### 2.6.4 SMT Pair 2 (Manual Mode)
- **Type:** String (symbol input)
- **Default:** ""
- **Purpose:** Second market symbol for manual SMT pair
- **Example:** "CME_MINI:YM1!" (E-mini Dow)
- **Validation:** Must be valid TradingView symbol
- **Impact:** Only used when SMT Mode = "Manual"

#### 2.6.5 SMT Inverse Correlation
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Indicates if SMT pair has inverse correlation
- **Behavior:** 
  - When ON, divergence detection accounts for inverse relationship
  - Ignored if SMT Mode = "Automatic"
- **Impact:** Affects divergence calculation logic

#### 2.6.6 SMT Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"
- **Purpose:** Line style for SMT divergence lines
- **Impact:** Visual customization

#### 2.6.7 SMT Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 2
- **Purpose:** Thickness of SMT lines
- **Impact:** Visibility

#### 2.6.8 SMT Line Color
- **Type:** Color picker
- **Default:** Cyan or teal
- **Purpose:** Color for SMT divergence lines
- **Impact:** Visual customization

#### 2.6.9 Show SMT Labels
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display divergence labels on chart
- **Impact:** Visual identification of divergence events

#### 2.6.10 SMT Label Size
- **Type:** Enum (dropdown)
- **Options:** "Tiny", "Small", "Normal", "Large", "Huge"
- **Default:** "Normal"
- **Purpose:** Size of SMT divergence labels
- **Impact:** Visibility

#### 2.6.11 SMT Alerts Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable alerts when SMT divergence is detected
- **Behavior:** Requires main Alerts toggle to be ON
- **Impact:** Notification when divergence occurs

### 2.7 Info Table Settings

#### 2.7.1 Show Info Table
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display information table on chart
- **Behavior:** Shows current fractal model state and settings
- **Impact:** Provides quick reference information

#### 2.7.2 Info Table Position
- **Type:** Enum (dropdown)
- **Options:** "Top Left", "Top Right", "Top Center", "Bottom Left", "Bottom Right", "Bottom Center", "On Chart"
- **Default:** "Top Left"
- **Purpose:** Location of info table on chart
- **Behavior:** 
  - Corner positions: Fixed table overlay
  - "On Chart": Displays below HTF candles
- **Impact:** Chart layout and readability

#### 2.7.3 Info Table Size
- **Type:** Enum (dropdown)
- **Options:** "Small", "Normal", "Large"
- **Default:** "Normal"
- **Purpose:** Size of info table text and cells
- **Impact:** Readability vs. chart space

#### 2.7.4 Show Table Borders
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display borders around table cells
- **Impact:** Visual clarity

#### 2.7.5 Show Fractal Pairing Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display current HTF-LTF pairing in table
- **Impact:** Quick reference for active timeframe relationship

#### 2.7.6 Show Time to Close
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display countdown to next HTF candle close
- **Behavior:** Shows time remaining until current HTF candle closes
- **Impact:** Helps time entries/exits relative to HTF structure

#### 2.7.7 Show Bias Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display current bias setting in table
- **Impact:** Quick reference for active bias filter

#### 2.7.8 Show Time Filter Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display active time filter windows in table
- **Behavior:** Shows which time filters are enabled and their windows
- **Impact:** Confirms time filter settings

### 2.8 Alert Settings

#### 2.8.1 Alert: Setup Formation
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Alert when new setup is detected
- **Behavior:** Triggers when valid C2/C3/C4 formation is confirmed
- **Impact:** Notification of new trading opportunity

#### 2.8.2 Alert: Setup Failure
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Alert when setup fails (label turns red)
- **Behavior:** Triggers when price returns to initial high/low without HTF swing
- **Impact:** Notification of invalidated setup

#### 2.8.3 Alert: Setup Consolidation
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Alert when setup enters consolidation (label turns orange)
- **Behavior:** Triggers when setup doesn't progress within next HTF candle
- **Impact:** Notification of potential range-bound market

#### 2.8.4 Alert: CISD Confirmation
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Alert when CISD is confirmed
- **Behavior:** Triggers when change in state of delivery is detected
- **Impact:** Early notification of potential setup formation

#### 2.8.5 Alert: T-Spot Reached
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Alert when price reaches T-Spot level
- **Behavior:** Triggers when price touches projected T-Spot area
- **Impact:** Notification of high-probability reversal/continuation zone

#### 2.8.6 Alert: Projection Level Reached
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Alert when price reaches any enabled projection level
- **Behavior:** Triggers when price touches projection line
- **Impact:** Notification of target level reached

#### 2.8.7 Alert: Projection Level (Specific)
- **Type:** Boolean (checkbox) - One for each projection level
- **Default:** OFF
- **Purpose:** Alert when specific projection level is reached
- **Behavior:** Allows selective alerts for specific projections
- **Impact:** Granular control over projection alerts

---

## 3. Core Concept & Framework

### 2.1 Fundamental Principle

The Fractal Model is based on the cyclical nature of price movements, where price alternates between large and small ranges. This cyclical behavior is fundamental to algorithmic price delivery, where markets move through phases of:

1. **Contraction/Consolidation:** Price moves in smaller ranges, building energy
2. **Expansion:** Price moves consistently in one direction with momentum, creating larger ranges
3. **Exhaustion:** Expansion completes, price seeks liquidity and reverses

**Expansion** occurs when price moves consistently in one direction with momentum. The model identifies moments when expansion is poised to occur by:

1. Combining **Higher Timeframe (HTF) closures** with
2. **Change in State of Delivery (CISD)** confirmation on the **Lower Timeframe (LTF)**

**Key Insight:** The HTF provides the structural narrative (the "what"), while the LTF provides the execution context (the "when" and "how"). By combining both, traders can identify high-probability expansion setups.

### 2.2 Timeframe Pairing System

The indicator operates on a **fractal pairing** concept where:
- **Higher Timeframe (HTF)**: Provides the structural context (e.g., 1H, 4H, 1D)
  - Defines the overall market structure
  - Establishes swing points and key levels
  - Provides the "big picture" narrative
- **Lower Timeframe (LTF)**: Shows detailed price action within HTF candles (e.g., 5m, 15m)
  - Reveals micro-structure within HTF context
  - Shows CISD formations in detail
  - Provides precise entry/exit timing

**Supported Pairings:**
- **Standard pairings:** Pre-defined common combinations
  - 1m-5m, 5m-15m, 5m-1H, 15m-1H, 15m-4H, 1H-4H, 4H-1D, 1D-1W
  - These represent typical fractal relationships where HTF is 3-12x the LTF
- **Custom pairings:** User-defined HTF-LTF combinations
  - Any valid timeframe combination where HTF > LTF
  - Must respect TradingView timeframe hierarchy
- **Automatic pairing:** System automatically selects appropriate HTF based on current chart timeframe
  - Uses predefined rules to select optimal HTF
  - Ensures proper fractal relationship

**Fractal Relationship Rules:**
- HTF should typically be 3-12x the LTF timeframe
- Common ratios: 3x, 4x, 5x, 6x, 12x
- Example: 5m chart with 1H HTF = 12x ratio
- Example: 15m chart with 1H HTF = 4x ratio

**Important Limitation:**
- If viewing a chart timeframe greater than the LTF (e.g., viewing 15m chart when 5m-1H is enabled), the indicator displays a warning message within the info table
- HTF candle plotting remains visible and functional
- LTF CISD detection will not render (cannot detect CISD on higher timeframe)
- LTF projections will not render
- This is a fundamental limitation: you cannot analyze LTF structure when viewing HTF or higher

### 2.3 Power of Three (PO3) Framework Integration

The HTF candles integrate the Power of Three framework, which identifies three critical phases within each HTF candle:

1. **Phase 1 (First Third):** Initial price action, establishment of direction
2. **Phase 2 (Middle Third):** Continuation or consolidation
3. **Phase 3 (Final Third):** Completion, exhaustion, or reversal preparation

This framework helps identify critical turning points and aligns with the fractal model's expansion/contraction cycles.

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

### 4.1 Setup Formation Process - Detailed Mechanics

**Step-by-Step Logic with Technical Details:**

#### Phase 1: HTF Candle Closure & Structure Analysis

1. **HTF Candle Closure Detection:**
   - Monitor HTF timeframe using `request.security()` with `lookahead=barmerge.lookahead_off`
   - Wait for HTF candle to close (confirmed, non-repainting)
   - Extract HTF candle data:
     - Open (O_HTF)
     - High (H_HTF)
     - Low (L_HTF)
     - Close (C_HTF)
     - Body size: `abs(C_HTF - O_HTF)`
     - Range: `H_HTF - L_HTF`
     - Body midpoint: `(O_HTF + C_HTF) / 2`
     - Range midpoint (Equilibrium): `(H_HTF + L_HTF) / 2`

2. **HTF Candle Classification:**
   - **Bullish HTF Candle:** C_HTF > O_HTF (green/blue)
   - **Bearish HTF Candle:** C_HTF < O_HTF (red)
   - **Doji HTF Candle:** C_HTF ≈ O_HTF (small body, potential reversal)
   - Determine HTF candle strength:
     - Strong: Body > 60% of range
     - Moderate: Body 40-60% of range
     - Weak: Body < 40% of range

3. **HTF Swing Point Identification:**
   - Compare current HTF candle with previous HTF candles
   - **Swing High:** HTF high is higher than previous N HTF highs (typically N=2)
   - **Swing Low:** HTF low is lower than previous N HTF lows (typically N=2)
   - These swing points become reference levels for setup validation

#### Phase 2: CISD Detection on Lower Timeframe

4. **LTF Price Action Monitoring:**
   - Within the current developing HTF candle, monitor all LTF candles
   - Track LTF candle sequence making up the HTF structure
   - Identify significant LTF candles (those contributing to HTF high/low)

5. **CISD Detection Logic:**

   **For Bullish CISD (Bearish to Bullish Change):**
   - Identify series of LTF candles making up a significant low
   - These candles should show bearish characteristics:
     - Lower lows and lower highs
     - Closes below opens (predominantly)
     - Building bearish momentum
   - **CISD Confirmation:** Detect when an LTF candle closes ABOVE its opening price
   - This close must occur after the significant low is established
   - The close above open signals shift from bearish delivery to bullish delivery
   - Mark this LTF candle as the CISD candle
   - CISD price level: Typically the low of the CISD candle or the low of the preceding bearish sequence

   **For Bearish CISD (Bullish to Bearish Change):**
   - Identify series of LTF candles making up a significant high
   - These candles should show bullish characteristics:
     - Higher highs and higher lows
     - Closes above opens (predominantly)
     - Building bullish momentum
   - **CISD Confirmation:** Detect when an LTF candle closes BELOW its opening price
   - This close must occur after the significant high is established
   - The close below open signals shift from bullish delivery to bearish delivery
   - Mark this LTF candle as the CISD candle
   - CISD price level: Typically the high of the CISD candle or the high of the preceding bullish sequence

6. **Early CISD Detection (Optional):**
   - If "Show Early CISD" is enabled:
   - Monitor for potential CISD before full confirmation
   - Look for initial signs of orderflow change:
     - For bullish: First LTF candle with close > open after bearish sequence
     - For bearish: First LTF candle with close < open after bullish sequence
   - Mark with distinct color/style (not confirmed yet)
   - Early CISD becomes confirmed CISD when full criteria met

#### Phase 3: Label Assignment (C2/C3/C4)

7. **C2 Identification (First Structural Point):**
   - **For Bullish Setup:**
     - C2 is the first significant swing low AFTER the bullish CISD
     - This low should be higher than or equal to the CISD low
     - C2 represents the first structural point in the new bullish formation
     - C2 price = Low of the C2 LTF candle
   - **For Bearish Setup:**
     - C2 is the first significant swing high AFTER the bearish CISD
     - This high should be lower than or equal to the CISD high
     - C2 represents the first structural point in the new bearish formation
     - C2 price = High of the C2 LTF candle

8. **C3 Identification (Second Structural Point):**
   - **For Bullish Setup:**
     - C3 is the first significant swing high after C2
     - This high should be higher than C2 (showing upward progression)
     - C3 represents the second structural point confirming bullish momentum
     - C3 price = High of the C3 LTF candle
   - **For Bearish Setup:**
     - C3 is the first significant swing low after C2
     - This low should be lower than C2 (showing downward progression)
     - C3 represents the second structural point confirming bearish momentum
     - C3 price = Low of the C3 LTF candle

9. **C4 Identification (Third Structural Point - Optional):**
   - **For Bullish Setup:**
     - C4 is the next significant swing low after C3
     - Should maintain higher low structure (C4 > C2 ideally)
     - C4 represents continuation or pullback point
     - C4 price = Low of the C4 LTF candle
   - **For Bearish Setup:**
     - C4 is the next significant swing high after C3
     - Should maintain lower high structure (C4 < C2 ideally)
     - C4 represents continuation or pullback point
     - C4 price = High of the C4 LTF candle
   - **Note:** C4 may not always form. Setup can be valid with just C2 and C3.

10. **Label State Initialization:**
    - All labels (C2, C3, C4) start in **Gray** state
    - Gray indicates: Setup is valid, all projections active
    - Labels are placed at their respective price levels
    - Text format: "C2", "C3", "C4" with appropriate size

#### Phase 4: Setup Validation & State Management

11. **Ongoing Setup Validation:**

    **Validation Checks (Per LTF Candle):**
    - Monitor price action relative to setup structure
    - Check if price is maintaining expected structure:
      - Bullish: Higher lows, higher highs
      - Bearish: Lower highs, lower lows
    - Verify price hasn't invalidated the setup

12. **Failure Detection Logic:**

    **Bullish Setup Failure:**
    - **Condition 1:** Price returns to or breaks below the initial low (CISD low or C2 low, whichever is lower)
    - **Condition 2:** This return occurs WITHOUT forming a new HTF swing low
    - **HTF Swing Low Check:** Current HTF low must be lower than previous HTF low
    - **If both conditions met:** Setup fails
    - **Failure Actions:**
      - Change all labels (C2, C3, C4) to **Red** color
      - Stop plotting: All projections, Equilibrium lines, Liquidity Sweep markers, T-Spot markers
      - Keep labels visible for reference (now red)
      - Setup is considered invalidated

    **Bearish Setup Failure:**
    - **Condition 1:** Price returns to or breaks above the initial high (CISD high or C2 high, whichever is higher)
    - **Condition 2:** This return occurs WITHOUT forming a new HTF swing high
    - **HTF Swing High Check:** Current HTF high must be higher than previous HTF high
    - **If both conditions met:** Setup fails
    - **Failure Actions:**
      - Change all labels (C2, C3, C4) to **Red** color
      - Stop plotting: All projections, Equilibrium lines, Liquidity Sweep markers, T-Spot markers
      - Keep labels visible for reference (now red)
      - Setup is considered invalidated

13. **Consolidation Detection Logic:**

    **Consolidation Criteria:**
    - Setup has NOT failed (labels still gray)
    - Current HTF candle has closed (setup formation period complete)
    - Price has NOT progressed significantly:
      - **Bullish:** Price hasn't made substantial new highs
      - **Bearish:** Price hasn't made substantial new lows
    - Market appears range-bound or paused
    - No clear continuation of expansion

    **Consolidation Actions:**
    - Change all labels (C2, C3, C4) to **Orange** color
    - Keep all projections and markers active (unlike failure)
    - Orange signals: Potential consolidation, slowdown, range-bound market
    - Setup is still valid but momentum has paused

    **Timing:** Consolidation check occurs after the HTF candle that defines the setup formation has closed

#### Phase 5: Projection & Level Calculation

14. **Projection Calculation (Only for Valid Setups - Gray Labels):**

    **Reference Points:**
    - Primary: C2 price level
    - Secondary: C3 price level
    - Tertiary: C4 price level (if exists)
    - CISD price level (for some projection types)

    **Body Projections Calculation:**
    - **Reference Range:** HTF candle body size = `abs(C_HTF - O_HTF)`
    - **For Bullish Setup:**
      - Projection_Level = C2_Price - (Body_Size × Multiplier)
      - Negative multipliers extend downward (opposite direction)
      - Example: C2 = 100, Body = 2, Multiplier = -1 → Projection = 98
    - **For Bearish Setup:**
      - Projection_Level = C2_Price + (Body_Size × Multiplier)
      - Negative multipliers extend upward (opposite direction)
      - Example: C2 = 100, Body = 2, Multiplier = -1 → Projection = 102

    **Wick Projections Calculation:**
    - **Reference Range:** HTF candle range = `H_HTF - L_HTF`
    - **For Bullish Setup:**
      - Use HTF Low as reference
      - Projection_Level = L_HTF - (Range × Multiplier)
    - **For Bearish Setup:**
      - Use HTF High as reference
      - Projection_Level = H_HTF + (Range × Multiplier)

    **Projection Rendering:**
    - Draw horizontal lines at each enabled projection level
    - Extend lines across the relevant price action area
    - Label each line with its multiplier value (if labels enabled)
    - Use specified line style, width, and color

15. **Equilibrium Calculation:**
    - **Current HTF EQ:** `(H_HTF + L_HTF) / 2`
    - **Previous HTF EQ:** `(Previous_H_HTF + Previous_L_HTF) / 2`
    - Draw horizontal line at equilibrium level
    - Extend across HTF candle period
    - Use specified line style and color

16. **T-Spot Calculation:**
    - **Methodology:** Based on TTrades' refined analysis
    - **General Approach:**
      - Analyze previous HTF candle wick formation
      - Consider current momentum and structure
      - Project where next HTF candle wick is likely to form
    - **For Bullish Setup:**
      - T-Spot typically projects above current price
      - Anticipates upper wick formation area
    - **For Bearish Setup:**
      - T-Spot typically projects below current price
      - Anticipates lower wick formation area
    - **Specific Algorithm:** (To be refined based on TTrades methodology)
      - May involve: Previous structure analysis, momentum measurement, Fibonacci relationships
    - Draw marker at T-Spot level
    - Use specified marker style, size, and color

17. **Formation Liquidity Marking:**
    - Identify previous HTF candles' highs and lows
    - Mark these as liquidity zones:
      - Previous HTF highs (for bearish setups - liquidity above)
      - Previous HTF lows (for bullish setups - liquidity below)
    - Draw horizontal lines at these levels
    - Use specified line style, width, and color
    - These represent "engineered liquidity pools" where price may seek stops

### 4.2 Setup Failure Conditions - Detailed

**Failure Detection Algorithm:**

```pseudocode
For each active setup:
    If setup is Bullish:
        Initial_Low = min(CISD_Low, C2_Low)
        Current_Price_Low = lowest price since setup formation
        
        If Current_Price_Low <= Initial_Low:
            Check HTF_Swing_Low:
                Previous_HTF_Low = HTF low from previous HTF candle
                Current_HTF_Low = HTF low from current HTF candle
                
                If Current_HTF_Low < Previous_HTF_Low:
                    HTF_Swing_Low_Formed = true
                Else:
                    HTF_Swing_Low_Formed = false
            
            If NOT HTF_Swing_Low_Formed:
                Setup_Failed = true
                Change_Labels_To_Red()
                Stop_Plotting_Projections()
    
    If setup is Bearish:
        Initial_High = max(CISD_High, C2_High)
        Current_Price_High = highest price since setup formation
        
        If Current_Price_High >= Initial_High:
            Check HTF_Swing_High:
                Previous_HTF_High = HTF high from previous HTF candle
                Current_HTF_High = HTF high from current HTF candle
                
                If Current_HTF_High > Previous_HTF_High:
                    HTF_Swing_High_Formed = true
                Else:
                    HTF_Swing_High_Formed = false
            
            If NOT HTF_Swing_High_Formed:
                Setup_Failed = true
                Change_Labels_To_Red()
                Stop_Plotting_Projections()
```

**Key Points:**
- Failure requires BOTH conditions: Price return AND no HTF swing formation
- If HTF swing forms, setup may still be valid (structure maintained at higher level)
- Failure is permanent for that setup (does not recover)
- Failed setups remain visible (red labels) for learning/reference

### 4.3 Consolidation Detection - Detailed

**Consolidation Detection Algorithm:**

```pseudocode
For each active setup (with Gray labels):
    Wait for HTF candle closure (setup formation period complete)
    
    If setup is Bullish:
        Progress_Check = Has price made significant new highs?
        Range_Check = Is price moving in a range?
        
        If NOT Progress_Check AND Range_Check:
            Consolidation_Detected = true
    
    If setup is Bearish:
        Progress_Check = Has price made significant new lows?
        Range_Check = Is price moving in a range?
        
        If NOT Progress_Check AND Range_Check:
            Consolidation_Detected = true
    
    If Consolidation_Detected:
        Change_Labels_To_Orange()
        Keep_All_Projections_Active()  // Unlike failure
        Setup_Still_Valid = true  // Unlike failure
```

**Consolidation vs. Failure:**
- **Consolidation (Orange):** Setup still valid, just paused. All projections remain active.
- **Failure (Red):** Setup invalidated. All projections stop. Setup is dead.

**Consolidation Recovery:**
- Setup can recover from consolidation (orange → gray) if momentum resumes
- Setup cannot recover from failure (red is permanent)

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

### 6.1 Time Filter Threshold - Detailed Logic

**Purpose:** Specify a timeframe threshold below which time filters will be applied. This prevents time filters from being applied on higher timeframes where they become less relevant.

**Detailed Logic:**
```pseudocode
// Get current chart timeframe in minutes
current_chart_timeframe_minutes = convert_timeframe_to_minutes(current_chart_timeframe)

// Get threshold timeframe in minutes
threshold_timeframe_minutes = convert_timeframe_to_minutes(time_filter_threshold_input)

// Determine if time filters should be active
if current_chart_timeframe_minutes <= threshold_timeframe_minutes:
    time_filters_active = true
    // Time filters will be applied to setup detection
else:
    time_filters_active = false
    // Time filters will be ignored, all setups shown regardless of time

// Example scenarios:
// Scenario 1: Chart = 5m, Threshold = 1H (60 minutes)
//   5 <= 60 → time_filters_active = true ✓

// Scenario 2: Chart = 1H, Threshold = 1H (60 minutes)
//   60 <= 60 → time_filters_active = true ✓

// Scenario 3: Chart = 4H, Threshold = 1H (60 minutes)
//   240 <= 60 → time_filters_active = false ✗
//   (Time filters ignored on 4H chart)
```

**Rationale:**
- Time filters are most useful on lower timeframes (1m, 5m, 15m)
- On higher timeframes (4H, 1D), time of day becomes less relevant
- Market structure and HTF context matter more than specific hours on higher timeframes
- Prevents unnecessary filtering on timeframes where it doesn't add value

### 6.2 Custom Time Filters - Detailed Mechanics

**Purpose:** Filter setup detection to specific time windows, allowing traders to focus on their preferred trading sessions (e.g., London open, New York open, Asian session).

**Time Filter Window Structure:**
```pseudocode
// Each time filter consists of:
time_filter = {
    enabled: boolean,           // Whether this filter is active
    start_time: time,          // Start time (HH:MM format)
    end_time: time,            // End time (HH:MM format)
    timezone_offset: integer   // UTC offset in hours
}

// Up to three time filters can be defined
time_filter_1 = {enabled: false, start: "02:00", end: "05:00", timezone: -5}
time_filter_2 = {enabled: false, start: "08:00", end: "11:00", timezone: -5}
time_filter_3 = {enabled: false, start: "14:00", end: "17:00", timezone: -5}
```

**Time Filter Application Logic:**
```pseudocode
// When detecting a new setup, check if it falls within active time filters
function should_show_setup_based_on_time_filters(setup_timestamp):
    // If time filters are not active (threshold check), show all setups
    if time_filters_active == false:
        return true  // Show setup
    
    // Get current time in specified timezone
    current_time_utc = get_current_time_utc()
    current_time_local = convert_timezone(current_time_utc, custom_timezone_offset)
    current_hour_minute = extract_hour_minute(current_time_local)
    
    // Check if current time falls within any enabled time filter
    setup_time_valid = false
    
    // Check Time Filter 1
    if time_filter_1.enabled:
        if is_time_in_window(current_hour_minute, time_filter_1.start, time_filter_1.end):
            setup_time_valid = true
    
    // Check Time Filter 2
    if time_filter_2.enabled:
        if is_time_in_window(current_hour_minute, time_filter_2.start, time_filter_2.end):
            setup_time_valid = true
    
    // Check Time Filter 3
    if time_filter_3.enabled:
        if is_time_in_window(current_hour_minute, time_filter_3.start, time_filter_3.end):
            setup_time_valid = true
    
    // If at least one filter is enabled, setup must fall within a window
    if any_filter_enabled:
        return setup_time_valid
    else:
        // No filters enabled, show all setups
        return true

// Helper function to check if time is within window
function is_time_in_window(check_time, window_start, window_end):
    // Handle normal case (start < end)
    if window_start < window_end:
        if check_time >= window_start AND check_time < window_end:
            return true
    
    // Handle wrap-around case (e.g., 22:00 to 02:00)
    else:  // window_start > window_end (crosses midnight)
        if check_time >= window_start OR check_time < window_end:
            return true
    
    return false

// Example: Time Filter 1: 02:00 to 05:00
// - 01:59 → false (before window)
// - 02:00 → true (start of window)
// - 03:30 → true (within window)
// - 04:59 → true (within window)
// - 05:00 → false (end of window, exclusive)
// - 05:01 → false (after window)

// Example: Time Filter with wrap-around: 22:00 to 02:00
// - 21:59 → false (before window)
// - 22:00 → true (start of window)
// - 23:30 → true (within window)
// - 00:00 → true (within window, after midnight)
// - 01:59 → true (within window)
// - 02:00 → false (end of window, exclusive)
// - 02:01 → false (after window)
```

**Multiple Time Filter Logic:**
```pseudocode
// When multiple time filters are enabled:
// Setup is shown if it falls within ANY of the enabled filters (OR logic)

// Example:
// Filter 1: 02:00-05:00 (enabled)
// Filter 2: 08:00-11:00 (enabled)
// Filter 3: 14:00-17:00 (disabled)

// Setup at 03:00 → Show (within Filter 1)
// Setup at 09:30 → Show (within Filter 2)
// Setup at 15:00 → Don't show (Filter 3 disabled, not in Filter 1 or 2)
// Setup at 06:00 → Don't show (not in any enabled filter)
```

**Custom Timezone Handling:**
```pseudocode
// Timezone conversion for time filters
custom_timezone_offset = user_input_timezone  // e.g., -5 for EST, +2 for CET

// Convert UTC time to custom timezone
function convert_to_custom_timezone(utc_timestamp, timezone_offset):
    // Add timezone offset (in hours)
    local_timestamp = utc_timestamp + (timezone_offset × 3600 seconds)
    return local_timestamp

// Example: New York Eastern Time (UTC-5)
// UTC time: 10:00
// Custom timezone: -5 hours
// Local time: 05:00 (5:00 AM)

// Example: London Time (UTC+0 in winter, UTC+1 in summer)
// UTC time: 10:00
// Custom timezone: +0 hours (winter) or +1 hour (summer)
// Local time: 10:00 (winter) or 11:00 (summer)

// Daylight Saving Time (DST) handling:
// User may need to adjust timezone offset manually for DST
// Or system may automatically detect DST based on date
if auto_dst_detection_enabled:
    current_date = get_current_date()
    if is_dst_period(current_date, timezone_location):
        timezone_offset = base_timezone_offset + 1  // Add 1 hour for DST
    else:
        timezone_offset = base_timezone_offset
```

**Time Filter Use Cases:**
```pseudocode
// Common trading session filters:

// London Session (European markets open)
london_filter = {
    enabled: true,
    start: "08:00",  // 8:00 AM London time
    end: "12:00",    // 12:00 PM London time
    timezone: 0      // UTC+0 (GMT)
}

// New York Session (US markets open)
new_york_filter = {
    enabled: true,
    start: "13:30",  // 1:30 PM London time (8:30 AM EST)
    end: "20:00",    // 8:00 PM London time (3:00 PM EST)
    timezone: -5     // UTC-5 (EST)
}

// Asian Session (Tokyo/Hong Kong)
asian_filter = {
    enabled: true,
    start: "00:00",   // Midnight London time
    end: "09:00",     // 9:00 AM London time
    timezone: +9      // UTC+9 (Tokyo time)
}

// Forex Overlap Periods (highest volatility)
london_new_york_overlap = {
    enabled: true,
    start: "13:30",   // When both sessions are open
    end: "16:00",
    timezone: 0
}
```

---

## 7. SMT Divergence Module (NEW Feature)

### 7.1 Purpose

Detect divergences between correlated or inversely correlated markets directly within the Fractal Model. SMT (Smart Money Tool) divergence helps identify when correlated markets are moving out of sync, which can signal potential reversals or continuations.

### 7.2 Mechanics - Detailed

**SMT Pair Selection:**

**Automatic Mode:**
```pseudocode
// System automatically selects most relevant SMT pair based on current symbol
// This requires a database of known correlations between markets

function auto_select_smt_pair(current_symbol):
    // Normalize symbol for matching
    symbol_upper = convert_to_uppercase(current_symbol)
    symbol_clean = remove_special_characters(symbol_upper)
    
    // Stock Index Futures Correlations:
    // S&P 500 related symbols
    if symbol_contains(symbol_clean, "ES") OR 
       symbol_contains(symbol_clean, "SPX") OR
       symbol_contains(symbol_clean, "SP500"):
        pair_1 = "CME_MINI:ES1!"  // E-mini S&P 500 futures
        pair_2 = "CME_MINI:YM1!"  // E-mini Dow Jones futures
        correlation_type = "POSITIVE"  // They typically move together
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: HIGH
        }
    
    // NASDAQ related symbols
    if symbol_contains(symbol_clean, "NQ") OR 
       symbol_contains(symbol_clean, "NASDAQ") OR
       symbol_contains(symbol_clean, "COMP"):
        pair_1 = "CME_MINI:NQ1!"  // E-mini NASDAQ futures
        pair_2 = "CME_MINI:ES1!"   // E-mini S&P 500 futures
        correlation_type = "POSITIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: HIGH
        }
    
    // Dow Jones related symbols
    if symbol_contains(symbol_clean, "YM") OR 
       symbol_contains(symbol_clean, "DOW") OR
       symbol_contains(symbol_clean, "DJI"):
        pair_1 = "CME_MINI:YM1!"  // E-mini Dow futures
        pair_2 = "CME_MINI:ES1!"   // E-mini S&P 500 futures
        correlation_type = "POSITIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: HIGH
        }
    
    // Russell 2000
    if symbol_contains(symbol_clean, "RTY") OR 
       symbol_contains(symbol_clean, "RUT"):
        pair_1 = "CME_MINI:RTY1!"  // E-mini Russell 2000
        pair_2 = "CME_MINI:ES1!"   // E-mini S&P 500
        correlation_type = "POSITIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: MEDIUM
        }
    
    // Forex Major Pairs Correlations:
    // EUR/USD
    if symbol_contains(symbol_clean, "EURUSD") OR
       symbol_contains(symbol_clean, "EUR_USD"):
        pair_1 = "FX:EURUSD"
        pair_2 = "FX:GBPUSD"  // GBP/USD (often correlated)
        correlation_type = "POSITIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: MEDIUM
        }
    
    // GBP/USD
    if symbol_contains(symbol_clean, "GBPUSD") OR
       symbol_contains(symbol_clean, "GBP_USD"):
        pair_1 = "FX:GBPUSD"
        pair_2 = "FX:EURUSD"
        correlation_type = "POSITIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: MEDIUM
        }
    
    // USD/JPY
    if symbol_contains(symbol_clean, "USDJPY") OR
       symbol_contains(symbol_clean, "USD_JPY"):
        pair_1 = "FX:USDJPY"
        pair_2 = "FX:EURUSD"  // Often inversely correlated
        correlation_type = "NEGATIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: LOW
        }
    
    // Crypto Correlations:
    if symbol_contains(symbol_clean, "BTC") OR
       symbol_contains(symbol_clean, "BITCOIN"):
        pair_1 = "BINANCE:BTCUSDT"
        pair_2 = "BINANCE:ETHUSDT"  // Ethereum
        correlation_type = "POSITIVE"
        return {
            pair_1: pair_1,
            pair_2: pair_2,
            correlation: correlation_type,
            confidence: MEDIUM
        }
    
    // If no match found:
    return {
        pair_1: NULL,
        pair_2: NULL,
        correlation: NULL,
        confidence: NONE,
        error: "No automatic pair found for symbol: " + current_symbol
    }

// Validate that auto-selected pairs are accessible
function validate_smt_pair(pair_info):
    if pair_info.pair_1 == NULL:
        return false
    
    // Test if pair_1 data is accessible
    test_data_1 = request_multi_timeframe_data(
        symbol = pair_info.pair_1,
        timeframe = current_timeframe,
        data_fields = [CLOSE_PRICE],
        lookahead_mode = DISABLED
    )
    
    if test_data_1 == NULL OR test_data_1[0] == NULL:
        return false
    
    // Test if pair_2 data is accessible
    test_data_2 = request_multi_timeframe_data(
        symbol = pair_info.pair_2,
        timeframe = current_timeframe,
        data_fields = [CLOSE_PRICE],
        lookahead_mode = DISABLED
    )
    
    if test_data_2 == NULL OR test_data_2[0] == NULL:
        return false
    
    return true
```

**Manual Mode:**
- User provides two symbol strings
- System validates symbols are accessible
- If invalid, display error and disable SMT

**Inverse Correlation:**
- When enabled, system inverts one market's price action for comparison
- Formula: `inverted_price = base_price - (current_price - base_price)`
- Only applies in Manual mode (Auto mode ignores this setting)

**Divergence Detection Algorithm:**
```pseudocode
// Get price data for both markets in the SMT pair
// Data must be synchronized to the same timeframe and time periods

// Request data for Pair 1 (typically the charted symbol or related market)
pair_1_data = request_multi_timeframe_data(
    symbol = smt_pair_1_symbol,
    timeframe = current_chart_timeframe,  // Use same timeframe as chart
    data_fields = [CLOSE_PRICE, HIGH_PRICE, LOW_PRICE],
    lookahead_mode = DISABLED,
    bar_count = SMT_LOOKBACK_PERIOD  // e.g., 50-100 bars for analysis
)

// Request data for Pair 2 (correlated market)
pair_2_data = request_multi_timeframe_data(
    symbol = smt_pair_2_symbol,
    timeframe = current_chart_timeframe,
    data_fields = [CLOSE_PRICE, HIGH_PRICE, LOW_PRICE],
    lookahead_mode = DISABLED,
    bar_count = SMT_LOOKBACK_PERIOD
)

// Validate data availability
if pair_1_data == NULL OR pair_2_data == NULL:
    show_warning("SMT data unavailable")
    disable_smt_divergence()
    return

// Handle inverse correlation if specified
// Inverse correlation means when Pair 1 goes up, Pair 2 goes down (and vice versa)
if inverse_correlation_enabled AND auto_smt_mode == false:
    // Invert Pair 2 data for comparison
    // Method: Use a base price and invert around it
    base_price = calculate_base_price(pair_2_data)  // e.g., average or first value
    
    for each price_point in pair_2_data:
        // Invert: distance from base becomes distance in opposite direction
        distance_from_base = price_point - base_price
        inverted_price = base_price - distance_from_base
        pair_2_data[price_point.index] = inverted_price
    
    // Note: If Auto SMT mode is enabled, inverse setting is ignored
    // because auto mode already determines correlation type

// Detect divergence
function detect_divergence(pair_1, pair_2, lookback_period=20):
    // Calculate recent trends
    pair_1_trend = calculate_trend(pair_1, lookback_period)
    pair_2_trend = calculate_trend(pair_2, lookback_period)
    
    // Bullish Divergence:
    // Pair 1 making lower lows, but Pair 2 making higher lows
    if pair_1_trend == "bearish" AND pair_2_trend == "bullish":
        divergence_type = "bullish_divergence"
        return true
    
    // Bearish Divergence:
    // Pair 1 making higher highs, but Pair 2 making lower highs
    if pair_1_trend == "bullish" AND pair_2_trend == "bearish":
        divergence_type = "bearish_divergence"
        return true
    
    return false

// Calculate trend
function calculate_trend(price_data, period):
    recent_highs = highest(price_data, period)
    recent_lows = lowest(price_data, period)
    
    current_high = highest(price_data, period/2)  // More recent
    current_low = lowest(price_data, period/2)
    
    if current_high > recent_highs AND current_low > recent_lows:
        return "bullish"
    elif current_high < recent_highs AND current_low < recent_lows:
        return "bearish"
    else:
        return "neutral"
```

**Divergence Visualization:**
```pseudocode
// Draw lines connecting divergence points
if divergence_detected:
    // Draw line on Pair 1 (current chart)
    plot_smt_line(
        x1 = divergence_start_bar,
        y1 = pair_1_price_at_start,
        x2 = current_bar,
        y2 = pair_1_price_current,
        color = divergence_color,
        style = smt_line_style,
        width = smt_line_width
    )
    
    // Draw label
    if show_smt_labels:
        plot_smt_label(
            x = current_bar,
            y = current_price,
            text = divergence_type,  // "Bullish Divergence" or "Bearish Divergence"
            size = smt_label_size,
            color = smt_label_color
        )
```

### 7.3 Integration with Fractal Model

**Divergence + Setup Interaction:**
- SMT divergence can confirm or contradict fractal model setups
- **Confirming:** Divergence aligns with setup direction (bullish divergence + bullish setup = stronger signal)
- **Contradicting:** Divergence opposes setup direction (may indicate weaker setup or upcoming reversal)

**Alert Integration:**
- SMT divergence alerts can be combined with setup alerts
- Alert when: Divergence detected AND setup formation occurs
- Provides multi-confirmation signals

### 7.4 Customization Options

- **Show SMT Lines:** Toggle visibility of divergence lines
- **SMT Alerts:** Enable alerts when divergence detected
- **Line Style:** Solid, dotted, dashed
- **Line Width:** 1-5 pixels
- **Line Color:** Customizable color picker
- **Show Labels:** Display divergence type labels
- **Label Size:** Tiny, Small, Normal, Large, Huge
- **Label Color:** Customizable

---

## 8. Information Display

### 8.1 Info Table - Detailed Implementation

**Purpose:** Display key information about current fractal model state in an easily accessible format.

**Information Displayed:**

**1. Fractal Pairing Information:**
```pseudocode
// Display current HTF-LTF relationship
fractal_pairing_text = "Fractal: " + ltf_timeframe + " / " + htf_timeframe
// Example: "Fractal: 5m / 1H"

// If automatic mode:
if fractal_pairing_mode == "Automatic":
    pairing_text += " (Auto)"
```

**2. Time Until Next HTF Candle Close:**
```pseudocode
// Calculate time remaining
current_time = time  // Current bar time
htf_candle_end_time = calculate_htf_candle_end_time(current_time, htf_timeframe)
time_remaining = htf_candle_end_time - current_time

// Format display
hours = floor(time_remaining / 3600000)  // Convert ms to hours
minutes = floor((time_remaining % 3600000) / 60000)
seconds = floor((time_remaining % 60000) / 1000)

time_to_close_text = "Time to Close: " + str(hours) + "h " + str(minutes) + "m " + str(seconds) + "s"
// Example: "Time to Close: 0h 23m 45s"
```

**3. Current Bias:**
```pseudocode
bias_text = "Bias: " + bias_selection
// Examples:
// "Bias: Neutral"
// "Bias: Bullish"
// "Bias: Bearish"
// "Bias: Auto Bias 1"
// "Bias: Auto Bias 2"
```

**4. Time Filter Status:**
```pseudocode
time_filter_text = "Time Filters: "
if time_filter_1_enable:
    time_filter_text += "[" + time_filter_1_start + "-" + time_filter_1_end + "] "
if time_filter_2_enable:
    time_filter_text += "[" + time_filter_2_start + "-" + time_filter_2_end + "] "
if time_filter_3_enable:
    time_filter_text += "[" + time_filter_3_start + "-" + time_filter_3_end + "] "
if not any_time_filters_enabled:
    time_filter_text += "None"

// Example: "Time Filters: [02:00-05:00] [08:00-11:00]"
```

**5. Warning Messages:**
```pseudocode
// Display warnings if applicable
if current_timeframe > ltf_timeframe:
    warning_text = "⚠ LTF analysis unavailable"
    display_warning(warning_text, color = color.orange)

if invalid_pairing:
    warning_text = "⚠ Invalid timeframe pairing"
    display_warning(warning_text, color = color.red)
```

**6. Active Setup Count:**
```pseudocode
active_setups_count = array.size(active_setups)
setups_text = "Active Setups: " + str(active_setups_count)
```

**Table Structure:**
```pseudocode
// Info table layout
table_data = [
    ["Fractal Pairing", fractal_pairing_text],
    ["Time to Close", time_to_close_text],
    ["Bias", bias_text],
    ["Time Filters", time_filter_text],
    ["Active Setups", setups_text]
]

// Add warnings if present
if warnings_exist:
    table_data.append(["⚠ Warnings", warning_text])
```

**Table Rendering:**
```pseudocode
// Create table object
if show_info_table:
    info_table = table.new(
        position = table_position,  // top_left, top_right, etc.
        columns = 2,
        rows = table_data.length,
        bgcolor = table_bg_color,
        border_width = if show_table_borders then 1 else 0,
        border_color = table_border_color
    )
    
    // Populate table
    for i = 0 to table_data.length - 1:
        // Header column
        table.cell(
            table_id = info_table,
            column = 0,
            row = i,
            text = table_data[i][0],
            text_color = header_text_color,
            bgcolor = header_bg_color
        )
        
        // Value column
        table.cell(
            table_id = info_table,
            column = 1,
            row = i,
            text = table_data[i][1],
            text_color = value_text_color,
            bgcolor = value_bg_color
        )
```

**On-Chart Display Option:**
```pseudocode
// Alternative: Display below HTF candles
if info_table_position == "On Chart":
    // Calculate position below HTF candles
    chart_y_position = calculate_chart_y_position()
    
    // Display as text labels or table below candles
    for each info_item in table_data:
        label.new(
            x = bar_index,
            y = chart_y_position,
            text = info_item[0] + ": " + info_item[1],
            style = label.style_label_left,
            size = info_table_size,
            color = info_text_color
        )
        chart_y_position -= label_spacing
```

**Customization Options:**

**Position Options:**
- Top Left: `position.top_left`
- Top Right: `position.top_right`
- Top Center: `position.top_center`
- Bottom Left: `position.bottom_left`
- Bottom Right: `position.bottom_right`
- Bottom Center: `position.bottom_center`
- On Chart: Custom positioning below HTF candles

**Size Options:**
- Small: Reduced font size, compact cells
- Normal: Standard font size
- Large: Increased font size for readability

**Border Options:**
- Show Borders: Display cell borders (grid lines)
- Hide Borders: Clean look without borders

**Selective Display:**
- User can enable/disable each information item
- Only selected items appear in table
- Reduces table size and clutter

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

### 12.1 Non-Repainting Behavior - Critical Implementation

**Critical Requirement:**
- Indicator must be **non-repainting** and **stable**
- Once a setup is formed within a time period, it must not change
- Levels and labels must remain unchanged within the given time period
- Only new time periods should generate new setups

**Implementation Requirements:**

**1. HTF Data Request:**
```pseudocode
// CRITICAL: Request HTF data with non-repainting settings
// The data request function must be configured to prevent lookahead bias
// Lookahead bias occurs when the system uses future data that wasn't available
// at the time the bar was forming, causing historical values to change

// Correct approach (non-repainting):
htf_data = request_multi_timeframe_data(
    symbol = current_symbol,
    timeframe = htf_timeframe,
    data_fields = [open, high, low, close, volume],
    lookahead_mode = DISABLED  // CRITICAL: Prevents repainting
)

// The lookahead_mode parameter controls whether the system can use
// data from bars that haven't closed yet. When DISABLED:
// - Only confirmed (closed) HTF candles are used
// - Historical values remain stable
// - No forward-looking bias

// NEVER use lookahead_mode = ENABLED
// This would cause repainting where:
// - Historical setups could change
// - Projections could shift
// - Labels could move
// - The indicator would be unreliable for backtesting
```

**2. Confirmed Candle Logic:**
```pseudocode
// The indicator must distinguish between:
// 1. Confirmed (closed) bars - historical data, stable
// 2. Unconfirmed (forming) bars - real-time data, may change

// Bar state detection:
current_bar_state = get_bar_state()

// Check if current bar is confirmed (closed)
if current_bar_state == BAR_CONFIRMED:
    // This bar has closed and will not change
    // Safe to:
    // - Detect new setups
    // - Calculate projections
    // - Update labels
    // - Store setup data permanently
    
    process_setup_detection()
    calculate_projections()
    update_labels()
    store_setup_data()

// Check if current bar is the last (real-time) bar
else if current_bar_state == BAR_LAST:
    // This is the currently forming bar
    // May update in real-time
    // Can show:
    // - Real-time HTF candle development
    // - Live projection updates
    // - Current setup status
    
    update_real_time_display()
    show_live_htf_candle()
    
// All other bars are historical (confirmed)
else:
    // Historical bars are always confirmed
    // Process normally
    process_historical_bar()
```

**3. Setup State Persistence:**
```pseudocode
// Store setup state in persistent data structures (non-repainting)
// These structures maintain setup information across bar updates

// Initialize persistent storage (only once, when indicator loads)
if indicator_initialized == false:
    setup_storage = create_persistent_storage()
    setup_storage.prices = create_array()
    setup_storage.bar_indices = create_array()
    setup_states = create_array()
    setup_directions = create_array()  // "bullish" or "bearish"
    setup_c2_prices = create_array()
    setup_c3_prices = create_array()
    setup_c4_prices = create_array()
    setup_cisd_prices = create_array()
    indicator_initialized = true

// Once setup is confirmed (on closed bar), store it permanently
if setup_confirmed == true AND current_bar_state == BAR_CONFIRMED:
    // Store all setup data
    add_to_array(setup_storage.prices, setup_reference_price)
    add_to_array(setup_storage.bar_indices, confirmed_bar_index)
    add_to_array(setup_states, "gray")  // Initial state
    add_to_array(setup_directions, setup_direction)
    add_to_array(setup_c2_prices, c2_price)
    
    if c3_exists:
        add_to_array(setup_c3_prices, c3_price)
    else:
        add_to_array(setup_c3_prices, NULL)
    
    if c4_exists:
        add_to_array(setup_c4_prices, c4_price)
    else:
        add_to_array(setup_c4_prices, NULL)
    
    add_to_array(setup_cisd_prices, cisd_price)
    
    // Mark setup as stored
    setup_stored = true
    
    // CRITICAL: This setup data will NEVER change for this bar
    // Even if new bars form, this historical setup remains fixed
    // Only the state (gray/red/orange) may change based on future price action
```

**4. Label Positioning and Management:**
```pseudocode
// Labels must be placed at confirmed bar indices
// Never use current bar index for historical labels
// Labels represent fixed points in time that cannot move

// For each stored setup:
for each setup in setup_storage:
    // Get confirmed bar index (fixed, never changes)
    label_x_position = setup.bar_index  // This is the bar where setup was confirmed
    
    // Get setup price level (fixed, never changes)
    label_y_position = setup.c2_price  // Or c3_price, c4_price depending on label
    
    // Label text is fixed
    label_text = determine_label_text(setup)  // "C2", "C3", or "C4"
    
    // Label color can change based on setup state
    // But the change only occurs when new bars confirm state change
    if setup.state == "gray":
        label_color = COLOR_GRAY  // Valid setup
    else if setup.state == "red":
        label_color = COLOR_RED  // Failed setup
    else if setup.state == "orange":
        label_color = COLOR_ORANGE  // Consolidation
    
    // Create or update label
    if setup.label_id == NULL:
        // Create new label
        setup.label_id = create_label(
            x = label_x_position,
            y = label_y_position,
            text = label_text,
            color = label_color,
            size = label_size_setting
        )
    else:
        // Update existing label (only color may change)
        update_label(
            label_id = setup.label_id,
            color = label_color  // Only color updates, position never changes
        )
    
    // CRITICAL RULES:
    // - Label x-position (bar index) NEVER changes
    // - Label y-position (price) NEVER changes
    // - Label text NEVER changes
    // - Only label color can change (gray -> red -> orange)
    // - Color change only happens on new confirmed bars
```

**5. Projection Calculation and Rendering:**
```pseudocode
// Projections are calculated from confirmed reference points
// Projection levels are fixed values that do not change
// Only the visual line extends to show current price position

// For each valid setup (state == "gray"):
for each setup in setup_storage:
    if setup.state == "gray":  // Only plot projections for valid setups
        // Get confirmed reference price (fixed, never changes)
        reference_price = setup.c2_price  // Primary reference
        
        // Get confirmed HTF body size (fixed, from the HTF candle when setup formed)
        confirmed_body_size = setup.htf_body_size
        
        // Get confirmed HTF range (fixed)
        confirmed_htf_range = setup.htf_range
        
        // Calculate each enabled projection level
        for each projection in enabled_projections:
            multiplier = projection.multiplier  // e.g., -1, -2, -2.5, -4, -4.5
            
            // Calculate projection level (fixed value)
            if projection.type == "body":
                if setup.direction == "bullish":
                    // Bullish: Projections extend downward
                    projection_level = reference_price - (confirmed_body_size × abs(multiplier))
                else:  // bearish
                    // Bearish: Projections extend upward
                    projection_level = reference_price + (confirmed_body_size × abs(multiplier))
            
            else if projection.type == "wick":
                if setup.direction == "bullish":
                    // Use HTF low as reference
                    projection_level = setup.htf_low - (confirmed_htf_range × abs(multiplier))
                else:  // bearish
                    // Use HTF high as reference
                    projection_level = setup.htf_high + (confirmed_htf_range × abs(multiplier))
            
            // CRITICAL: projection_level is a FIXED value
            // It is calculated once when setup is confirmed
            // It never changes, even as new bars form
            
            // Create or update projection line
            if setup.projection_line_ids[projection.id] == NULL:
                // Create new line
                line_id = create_line(
                    x1 = setup.bar_index,  // Start at setup bar (fixed)
                    y1 = projection_level,  // Fixed level
                    x2 = current_bar_index,  // Extends to current bar (updates)
                    y2 = projection_level,  // Same level
                    style = projection_line_style,
                    width = projection_line_width,
                    color = projection_line_color
                )
                setup.projection_line_ids[projection.id] = line_id
            else:
                // Update existing line (only extend x2, y2 stays same)
                update_line(
                    line_id = setup.projection_line_ids[projection.id],
                    x2 = current_bar_index,  // Extend to current bar
                    y2 = projection_level  // Level never changes
                )
            
            // Add label if enabled
            if show_projection_labels:
                label_text = format_multiplier(multiplier)  // e.g., "-1", "-2.5"
                if setup.projection_label_ids[projection.id] == NULL:
                    label_id = create_label(
                        x = setup.bar_index,
                        y = projection_level,
                        text = label_text,
                        color = projection_label_color,
                        size = projection_label_size
                    )
                    setup.projection_label_ids[projection.id] = label_id
```

**6. Testing Non-Repainting:**
```pseudocode
// Test procedure:
// 1. Load indicator on chart
// 2. Note position of labels and levels
// 3. Let chart update (new bars form)
// 4. Historical labels/levels should NOT move
// 5. Only new setups should appear
// 6. Failed setups (red) should remain red, not change back
```

### 12.2 Multi-Timeframe Handling - Detailed

**HTF-LTF Relationship Management:**
```pseudocode
// Get current chart timeframe
current_timeframe = timeframe.period  // e.g., "5", "15", "60", "240"

// Get HTF timeframe
htf_timeframe = user_selected_htf  // e.g., "60" for 1H

// Get LTF timeframe (from pairing)
ltf_timeframe = get_ltf_from_pairing(htf_timeframe)  // e.g., "5" for 5m

// Validate relationship
function validate_timeframe_pairing(current_tf, htf_tf, ltf_tf):
    // Check if current timeframe is compatible
    if current_tf > ltf_tf:
        // Warning: Cannot analyze LTF structure on higher timeframe
        show_warning("LTF analysis unavailable. Chart timeframe > LTF.")
        ltf_analysis_available = false
    else:
        ltf_analysis_available = true
    
    // Check if HTF > LTF
    if htf_tf <= ltf_tf:
        show_error("HTF must be greater than LTF")
        return false
    
    return true
```

**Timeframe Conversion:**
```pseudocode
// Convert timeframe strings to minutes
function timeframe_to_minutes(tf_string):
    if tf_string == "1":
        return 1
    elif tf_string == "5":
        return 5
    elif tf_string == "15":
        return 15
    elif tf_string == "60":
        return 60
    elif tf_string == "240":
        return 240
    elif tf_string == "D":
        return 1440  // 24 hours
    // ... etc
    
// Calculate ratio
htf_minutes = timeframe_to_minutes(htf_timeframe)
ltf_minutes = timeframe_to_minutes(ltf_timeframe)
current_minutes = timeframe_to_minutes(current_timeframe)

ratio = htf_minutes / ltf_minutes  // Should be 3-12 typically
```

**Warning Display:**
```pseudocode
// Display warning in info table
if current_minutes > ltf_minutes:
    warning_message = "⚠ LTF analysis unavailable (Chart > LTF)"
    display_in_info_table(warning_message, color = color.orange)
    
    // HTF candles still work
    plot_htf_candles()  // Continue
    
    // LTF features disabled
    disable_cisd_detection()
    disable_ltf_projections()
```

### 12.3 Performance Considerations - Optimization Strategies

**1. Array Management and Setup Storage:**
```pseudocode
// Use efficient data structures for managing multiple setups
// The indicator must track up to 40 historical setups simultaneously

// Initialize setup storage structure (persistent across bar updates)
if setup_storage_initialized == false:
    // Create main setup array
    active_setups = create_persistent_array()
    
    // Each setup object contains:
    setup_structure = {
        bar_index: integer,           // Bar where setup was confirmed
        setup_id: unique_identifier,   // Unique ID for this setup
        direction: string,             // "bullish" or "bearish"
        state: string,                 // "gray", "red", or "orange"
        c2_price: float,               // C2 price level
        c3_price: float,               // C3 price level (may be NULL)
        c4_price: float,               // C4 price level (may be NULL)
        cisd_price: float,             // CISD price level
        htf_body_size: float,          // HTF candle body size
        htf_range: float,              // HTF candle range
        htf_high: float,               // HTF candle high
        htf_low: float,                // HTF candle low
        projection_levels: array,     // Calculated projection levels
        label_ids: array,              // IDs of labels for this setup
        line_ids: array,               // IDs of lines for this setup
        created_timestamp: timestamp    // When setup was created
    }
    
    setup_storage_initialized = true

// Limit array size based on user input
max_setups = min(history_setups_input, 40)  // Cap at 40 maximum

// Manage setup array size
current_setup_count = get_array_size(active_setups)

if current_setup_count > max_setups:
    // Remove oldest setups (FIFO - First In First Out)
    setups_to_remove = current_setup_count - max_setups
    
    for i = 0 to setups_to_remove - 1:
        oldest_setup = remove_first_element(active_setups)
        
        // Clean up associated visual elements
        delete_all_labels(oldest_setup.label_ids)
        delete_all_lines(oldest_setup.line_ids)
        
        // Free memory
        destroy_setup_object(oldest_setup)

// When new setup is confirmed:
if new_setup_confirmed:
    // Add to array
    new_setup = create_setup_object(
        bar_index = confirmed_bar_index,
        direction = detected_direction,
        state = "gray",
        c2_price = confirmed_c2_price,
        // ... other setup data
    )
    
    add_to_array(active_setups, new_setup)
    
    // Ensure array doesn't exceed limit
    if get_array_size(active_setups) > max_setups:
        oldest_setup = remove_first_element(active_setups)
        cleanup_setup_visuals(oldest_setup)
```

**2. Calculation Caching:**
```pseudocode
// Cache expensive calculations
var float cached_htf_eq = na
var int cached_htf_bar = na

// Only recalculate when HTF candle closes
if htf_candle_closed:
    cached_htf_eq = (H_HTF + L_HTF) / 2
    cached_htf_bar = bar_index
else:
    // Use cached value
    current_eq = cached_htf_eq
```

**3. Conditional Rendering:**
```pseudocode
// Only render visible elements
if show_htf_candles:
    plot_htf_candles()

if show_projections AND setup_state == "gray":
    plot_projections()

// Don't calculate if not displayed
if not show_smt:
    skip_smt_calculations()
```

**4. Multi-Timeframe Data Request Optimization:**
```pseudocode
// Optimize data requests to minimize platform API calls
// Each request has overhead, so batch requests when possible

// INEFFICIENT APPROACH (Multiple separate requests):
// This would make 5 separate API calls:
O_HTF = request_multi_timeframe_data(symbol, htf, OPEN_PRICE, ...)
H_HTF = request_multi_timeframe_data(symbol, htf, HIGH_PRICE, ...)
L_HTF = request_multi_timeframe_data(symbol, htf, LOW_PRICE, ...)
C_HTF = request_multi_timeframe_data(symbol, htf, CLOSE_PRICE, ...)
V_HTF = request_multi_timeframe_data(symbol, htf, VOLUME, ...)

// EFFICIENT APPROACH (Single batched request):
// Make one API call that returns all needed data
htf_data = request_multi_timeframe_data(
    symbol = current_symbol,
    timeframe = htf_timeframe,
    data_fields = [
        OPEN_PRICE,
        HIGH_PRICE,
        LOW_PRICE,
        CLOSE_PRICE,
        VOLUME
    ],
    lookahead_mode = DISABLED
)

// Extract all data from single response
O_HTF = htf_data[0]  // Open price
H_HTF = htf_data[1]  // High price
L_HTF = htf_data[2]  // Low price
C_HTF = htf_data[3]  // Close price
V_HTF = htf_data[4]  // Volume (if needed)

// Additional optimization: Cache previous HTF candle data
// Only request new data when HTF candle closes
if htf_candle_closed_since_last_check:
    // HTF candle just closed, request new data
    htf_data = request_multi_timeframe_data(...)
    cache_htf_data(htf_data)
    update_htf_candle_close_timestamp()
else:
    // HTF candle still forming, use cached data
    htf_data = get_cached_htf_data()

// For previous HTF candle (for Previous Candle EQ):
// Request once and cache
if previous_htf_data_cached == false:
    previous_htf_data = request_multi_timeframe_data(
        symbol = current_symbol,
        timeframe = htf_timeframe,
        data_fields = [HIGH_PRICE, LOW_PRICE],
        bar_offset = 1,  // Previous bar
        lookahead_mode = DISABLED
    )
    cache_previous_htf_data(previous_htf_data)
    previous_htf_data_cached = true

// Performance benefits:
// - Reduces API calls by 80% (5 calls -> 1 call)
// - Faster indicator execution
// - Lower server load
// - Better user experience
```

**5. Label and Visual Element Management:**
```pseudocode
// Efficiently manage visual elements (labels, lines, markers)
// Charting platforms have limits on the number of visual elements
// Must balance visual clarity with performance

// Initialize visual element tracking
if visual_elements_initialized == false:
    all_label_ids = create_tracking_array()
    all_line_ids = create_tracking_array()
    all_marker_ids = create_tracking_array()
    max_labels_allowed = 500  // Platform limit (adjust based on platform)
    max_lines_allowed = 1000  // Platform limit
    visual_elements_initialized = true

// Strategy: Use lines instead of labels where possible
// - Lines are more efficient for drawing levels
// - Labels are better for text annotations
// - Markers are good for point identification

// Label management:
current_label_count = get_array_size(all_label_ids)

if current_label_count >= max_labels_allowed:
    // Remove oldest labels (FIFO)
    labels_to_remove = current_label_count - max_labels_allowed + 10  // Remove extra for buffer
    
    for i = 0 to labels_to_remove - 1:
        oldest_label_id = remove_first_element(all_label_ids)
        delete_label(oldest_label_id)

// Line management:
current_line_count = get_array_size(all_line_ids)

if current_line_count >= max_lines_allowed:
    // Remove oldest lines
    lines_to_remove = current_line_count - max_lines_allowed + 50  // Buffer
    
    for i = 0 to lines_to_remove - 1:
        oldest_line_id = remove_first_element(all_line_ids)
        delete_line(oldest_line_id)

// When creating new visual elements:
function create_setup_labels(setup):
    // C2 label
    c2_label_id = create_label(
        x = setup.bar_index,
        y = setup.c2_price,
        text = "C2",
        color = get_state_color(setup.state),
        size = label_size_setting
    )
    add_to_array(all_label_ids, c2_label_id)
    add_to_array(setup.label_ids, c2_label_id)
    
    // C3 label (if exists)
    if setup.c3_price != NULL:
        c3_label_id = create_label(
            x = setup.bar_index,
            y = setup.c3_price,
            text = "C3",
            color = get_state_color(setup.state),
            size = label_size_setting
        )
        add_to_array(all_label_ids, c3_label_id)
        add_to_array(setup.label_ids, c3_label_id)
    
    // C4 label (if exists)
    if setup.c4_price != NULL:
        c4_label_id = create_label(
            x = setup.bar_index,
            y = setup.c4_price,
            text = "C4",
            color = get_state_color(setup.state),
            size = label_size_setting
        )
        add_to_array(all_label_ids, c4_label_id)
        add_to_array(setup.label_ids, c4_label_id)

// When setup is removed, clean up all its visual elements
function cleanup_setup_visuals(setup):
    // Delete all labels
    for each label_id in setup.label_ids:
        delete_label(label_id)
        remove_from_array(all_label_ids, label_id)
    
    // Delete all lines
    for each line_id in setup.line_ids:
        delete_line(line_id)
        remove_from_array(all_line_ids, line_id)
    
    // Delete all markers
    for each marker_id in setup.marker_ids:
        delete_marker(marker_id)
        remove_from_array(all_marker_ids, marker_id)
```

**6. Performance Testing:**
```pseudocode
// Test scenarios:
// 1. Maximum history (40 setups)
// 2. All features enabled
// 3. Multiple timeframe pairings
// 4. Long chart history (1000+ bars)
// 5. Real-time updates

// Performance targets:
// - Chart load time: < 2 seconds
// - Real-time update: < 100ms
// - Memory usage: Reasonable (monitor with TradingView tools)
```

### 12.4 Error Handling & Edge Cases

**1. Invalid Timeframe Pairings:**
```pseudocode
function handle_invalid_pairing():
    if htf_timeframe <= ltf_timeframe:
        show_error("HTF must be greater than LTF")
        disable_indicator()
        return false
    return true
```

**2. Insufficient Data:**
```pseudocode
function check_data_availability():
    required_bars = calculate_required_bars(htf_timeframe, history_setups)
    
    if bar_index < required_bars:
        show_message("Insufficient data. Need " + str(required_bars) + " bars.")
        // Show partial indicator or wait
        return false
    return true
```

**3. Market Gaps:**
```pseudocode
// Handle gaps in price data
if na(close) or na(open):
    // Skip this bar or use previous values
    use_previous_values()
```

**4. SMT Pair Unavailability:**
```pseudocode
function check_smt_pair_availability():
    try:
        test_data = request.security(smt_pair_1, timeframe, close, ...)
        if na(test_data):
            show_warning("SMT Pair 1 data unavailable")
            disable_smt()
            return false
    except:
        show_error("SMT Pair 1 symbol invalid")
        disable_smt()
        return false
    return true
```

### 12.5 Platform Compatibility & Requirements

**Platform Version Requirements:**
- Target platform: TradingView (or specified charting platform)
- Script language version: Latest stable version
- Use modern syntax and functions
- Avoid deprecated functions that may be removed in future versions

**Required Platform Functions:**

**1. Multi-Timeframe Data Access:**
```pseudocode
// Function to request data from different timeframes
request_multi_timeframe_data(
    symbol,
    timeframe,
    data_fields,
    lookahead_mode,
    bar_offset
)
// This is essential for HTF candle analysis
```

**2. Array/Collection Management:**
```pseudocode
// Functions for managing setup storage
create_array()
add_to_array(array, element)
remove_from_array(array, index)
get_array_element(array, index)
get_array_size(array)
clear_array(array)
// Required for managing multiple historical setups
```

**3. Visual Element Creation:**
```pseudocode
// Label creation and management
create_label(x, y, text, color, size, style)
update_label(label_id, properties)
delete_label(label_id)
get_label_properties(label_id)

// Line creation and management
create_line(x1, y1, x2, y2, color, width, style)
update_line(line_id, properties)
delete_line(line_id)
extend_line(line_id, new_x2, new_y2)

// Marker/Shape creation
create_marker(x, y, shape, color, size)
update_marker(marker_id, properties)
delete_marker(marker_id)
```

**4. Bar State Detection:**
```pseudocode
// Functions to determine bar state
get_bar_state()  // Returns: CONFIRMED, FORMING, REAL_TIME
is_bar_confirmed(bar_index)
is_bar_last()
is_bar_real_time()
get_current_bar_index()
get_bar_time(bar_index)
```

**5. Table Creation (for Info Table):**
```pseudocode
// Table creation and management
create_table(position, columns, rows, properties)
set_table_cell(table_id, column, row, text, properties)
update_table_cell(table_id, column, row, properties)
delete_table(table_id)
```

**6. Price Data Access:**
```pseudocode
// Access current chart price data
get_open_price(bar_index)
get_high_price(bar_index)
get_low_price(bar_index)
get_close_price(bar_index)
get_volume(bar_index)
get_time(bar_index)
```

**Compatibility Testing Requirements:**

**1. Chart Type Compatibility:**
```pseudocode
// Test on all available chart types:
test_chart_types = [
    "Candlestick",
    "Line",
    "Area",
    "Baseline",
    "Heikin Ashi",
    "Renko",
    "Point & Figure"
]

// The indicator should work correctly on all chart types
// Visual elements may need adjustment for different chart types
for each chart_type in test_chart_types:
    test_indicator_on_chart_type(chart_type)
    verify_visual_elements_render_correctly()
    verify_calculations_are_accurate()
```

**2. Asset Type Compatibility:**
```pseudocode
// Test on different asset classes:
test_assets = [
    "Stock Indices" (e.g., SPX, NASDAQ),
    "Stock Futures" (e.g., ES, NQ, YM),
    "Forex Pairs" (e.g., EURUSD, GBPUSD),
    "Cryptocurrencies" (e.g., BTC, ETH),
    "Commodities" (e.g., Gold, Oil),
    "Bonds" (e.g., 10Y Treasury)
]

// Each asset class may have different:
// - Trading hours
// - Price precision
// - Volume characteristics
// - Gap behavior

for each asset_class in test_assets:
    test_indicator_on_asset_class(asset_class)
    verify_time_filter_handling()
    verify_price_precision_handling()
    verify_gap_handling()
```

**3. Timeframe Compatibility:**
```pseudocode
// Test on all common timeframes:
test_timeframes = [
    "1 minute",
    "5 minutes",
    "15 minutes",
    "30 minutes",
    "1 hour",
    "4 hours",
    "1 day",
    "1 week"
]

// Different timeframes may have:
// - Different data availability
// - Different update frequencies
// - Different precision requirements

for each timeframe in test_timeframes:
    test_indicator_on_timeframe(timeframe)
    verify_htf_ltf_pairing_works()
    verify_calculations_are_accurate()
    verify_performance_is_acceptable()
```

**4. Mobile App Compatibility:**
```pseudocode
// If platform has mobile app:
if platform_has_mobile_app:
    test_on_mobile_devices = [
        "iOS iPhone",
        "iOS iPad",
        "Android Phone",
        "Android Tablet"
    ]
    
    for each device in test_on_mobile_devices:
        test_indicator_on_device(device)
        verify_visual_elements_display_correctly()
        verify_touch_interactions_work()
        verify_performance_is_acceptable()
        verify_battery_usage_is_reasonable()
```

**5. Browser Compatibility:**
```pseudocode
// Test on different web browsers:
test_browsers = [
    "Chrome",
    "Firefox",
    "Safari",
    "Edge"
]

// Different browsers may have:
// - Different rendering engines
// - Different performance characteristics
// - Different JavaScript implementations

for each browser in test_browsers:
    test_indicator_in_browser(browser)
    verify_rendering_is_correct()
    verify_performance_is_acceptable()
    verify_no_console_errors()
```

**6. Data Availability Edge Cases:**
```pseudocode
// Test scenarios with limited data:
test_scenarios = [
    "New symbol with limited history",
    "Symbol with gaps in data",
    "Symbol with extended market closure",
    "Symbol with data feed interruption",
    "Symbol with irregular trading hours"
]

// Indicator should handle these gracefully:
for each scenario in test_scenarios:
    test_indicator_with_scenario(scenario)
    verify_graceful_degradation()
    verify_error_messages_are_clear()
    verify_no_crashes_or_errors()
```

---

## 13. Calculation Logic Details

### 13.1 HTF Candle Calculation - Detailed Implementation

**Data Request Process:**
```pseudocode
// Request higher timeframe data from the charting platform
// This function retrieves data from a different timeframe than the current chart

// Function signature:
htf_data = request_multi_timeframe_data(
    symbol = current_chart_symbol,  // The symbol being charted
    target_timeframe = htf_timeframe,  // e.g., "60" for 1H, "240" for 4H
    data_fields = [
        OPEN_PRICE,    // O_HTF - Opening price of HTF candle
        HIGH_PRICE,    // H_HTF - Highest price in HTF candle
        LOW_PRICE,     // L_HTF - Lowest price in HTF candle
        CLOSE_PRICE,   // C_HTF - Closing price of HTF candle
        VOLUME         // V_HTF - Volume (optional, may not be used)
    ],
    lookahead_mode = DISABLED,  // CRITICAL: Prevents repainting
    gaps_handling = GAPS_NONE  // How to handle gaps in data
)

// The function returns an array where:
// htf_data[0] = O_HTF (open)
// htf_data[1] = H_HTF (high)
// htf_data[2] = L_HTF (low)
// htf_data[3] = C_HTF (close)
// htf_data[4] = V_HTF (volume, if requested)

// Important considerations:
// 1. The data is aligned to the current bar's time
//    - If current bar is at 10:30 on 5m chart
//    - And HTF is 1H, it returns the 1H candle that contains 10:30
// 2. Data is only from confirmed (closed) HTF candles when lookahead_mode = DISABLED
// 3. If HTF candle hasn't closed yet, returns data from previous closed HTF candle
// 4. Multiple calls should be batched when possible for performance
```

**HTF Candle Metrics Calculation:**
```pseudocode
// Extract HTF candle data
O_HTF = htf_data[0]  // Open
H_HTF = htf_data[1]  // High
L_HTF = htf_data[2]  // Low
C_HTF = htf_data[3]  // Close

// Calculate derived metrics
HTF_Body_Size = abs(C_HTF - O_HTF)
HTF_Range = H_HTF - L_HTF
HTF_Body_Midpoint = (O_HTF + C_HTF) / 2
HTF_Range_Midpoint = (H_HTF + L_HTF) / 2  // Equilibrium

// Body percentage (strength indicator)
HTF_Body_Percentage = (HTF_Body_Size / HTF_Range) × 100

// Candle direction
HTF_Is_Bullish = C_HTF > O_HTF
HTF_Is_Bearish = C_HTF < O_HTF
HTF_Is_Doji = abs(C_HTF - O_HTF) < (HTF_Range × 0.1)  // Body < 10% of range
```

**HTF Candle Plotting Logic:**
```pseudocode
// Determine which LTF candles belong to current HTF candle
Current_HTF_Time = time in HTF timeframe
HTF_Candle_Start_Time = start of current HTF candle period
HTF_Candle_End_Time = end of current HTF candle period

// For each LTF bar:
If LTF_bar_time >= HTF_Candle_Start_Time AND LTF_bar_time < HTF_Candle_End_Time:
    This LTF bar belongs to current HTF candle
    Plot HTF candle with:
        x_position = current_bar_index + offset
        open = O_HTF
        high = H_HTF
        low = L_HTF
        close = C_HTF
        width = candle_size
        colors = based on bullish/bearish
```

**HTF Candle Offset Calculation:**
```pseudocode
// Offset shifts HTF candles left/right
offset_bars = user_input_offset  // -50 to +50

// Calculate x-coordinate for HTF candle
htf_candle_x = current_bar_index + offset_bars

// Ensure offset doesn't go beyond chart boundaries
if htf_candle_x < 0:
    htf_candle_x = 0
if htf_candle_x > last_bar_index:
    htf_candle_x = last_bar_index
```

**HTF Lookback History:**
```pseudocode
// Limit number of HTF candles plotted
max_htf_candles = user_input_lookback_history  // 1 to 100

// Calculate how many HTF candles to plot
htf_candles_to_plot = min(max_htf_candles, available_htf_candles)

// Plot only the most recent N HTF candles
for i = 0 to htf_candles_to_plot - 1:
    htf_candle_index = current_htf_index - i
    if htf_candle_index >= 0:
        plot_htf_candle(htf_candle_index)
```

### 13.2 CISD Detection Logic - Detailed Algorithm

**Bullish CISD Detection:**
```pseudocode
// Monitor LTF candles within current HTF candle
ltf_candles_in_htf = get_ltf_candles_in_htf_period()

// Identify bearish sequence (making significant low)
bearish_sequence = []
for each ltf_candle in ltf_candles_in_htf:
    if ltf_candle.close < ltf_candle.open:  // Bearish candle
        bearish_sequence.append(ltf_candle)
    else:
        // Check if we've established a significant low
        if length(bearish_sequence) >= 2:  // At least 2 bearish candles
            significant_low = min(low for all candles in bearish_sequence)
            
            // Look for CISD confirmation
            if ltf_candle.close > ltf_candle.open:  // Bullish close
                // This is the CISD candle
                cisd_candle = ltf_candle
                cisd_price = significant_low  // Or cisd_candle.low
                cisd_confirmed = true
                break

// Mark CISD on chart
if cisd_confirmed:
    plot_cisd_marker(
        x = cisd_candle.bar_index,
        y = cisd_price,
        color = bullish_cisd_color,
        style = cisd_line_style,
        width = cisd_line_width
    )
```

**Bearish CISD Detection:**
```pseudocode
// Monitor LTF candles within current HTF candle
ltf_candles_in_htf = get_ltf_candles_in_htf_period()

// Identify bullish sequence (making significant high)
bullish_sequence = []
for each ltf_candle in ltf_candles_in_htf:
    if ltf_candle.close > ltf_candle.open:  // Bullish candle
        bullish_sequence.append(ltf_candle)
    else:
        // Check if we've established a significant high
        if length(bullish_sequence) >= 2:  // At least 2 bullish candles
            significant_high = max(high for all candles in bullish_sequence)
            
            // Look for CISD confirmation
            if ltf_candle.close < ltf_candle.open:  // Bearish close
                // This is the CISD candle
                cisd_candle = ltf_candle
                cisd_price = significant_high  // Or cisd_candle.high
                cisd_confirmed = true
                break

// Mark CISD on chart
if cisd_confirmed:
    plot_cisd_marker(
        x = cisd_candle.bar_index,
        y = cisd_price,
        color = bearish_cisd_color,
        style = cisd_line_style,
        width = cisd_line_width
    )
```

**Early CISD Detection:**
```pseudocode
// Similar to above but with less strict criteria
// First sign of potential orderflow change

// For Bullish Early CISD:
if first_ltf_candle.close > first_ltf_candle.open after bearish_sequence:
    early_cisd_detected = true
    plot_early_cisd_marker(
        color = early_cisd_color,  // Distinct from confirmed
        style = dotted_line  // Different style
    )

// Early CISD becomes confirmed when full criteria met
```

### 13.3 Equilibrium Calculation - Detailed

**Current HTF Equilibrium:**
```pseudocode
// Simple midpoint calculation
current_htf_eq = (H_HTF + L_HTF) / 2

// Plot equilibrium line
plot_equilibrium_line(
    y = current_htf_eq,
    style = equilibrium_line_style,
    width = equilibrium_line_width,
    color = equilibrium_color,
    extend = extend_right  // Extend across HTF candle period
)
```

**Previous HTF Equilibrium:**
```pseudocode
// Get previous HTF candle data
previous_htf_data = request.security(symbol, htf_timeframe, [
    high[1],  // Previous HTF high
    low[1]    // Previous HTF low
], lookahead=barmerge.lookahead_off)

previous_htf_high = previous_htf_data[0]
previous_htf_low = previous_htf_data[1]

previous_htf_eq = (previous_htf_high + previous_htf_low) / 2

// Plot previous equilibrium
if show_previous_candle_eq:
    plot_equilibrium_line(
        y = previous_htf_eq,
        style = previous_eq_line_style,
        width = previous_eq_line_width,
        color = previous_eq_color,
        extend = extend_right
    )
```

**Premium and Discount Zones:**
```pseudocode
// Premium zone (upper 50% of HTF range)
premium_zone_top = H_HTF
premium_zone_bottom = current_htf_eq

// Discount zone (lower 50% of HTF range)
discount_zone_top = current_htf_eq
discount_zone_bottom = L_HTF

// Conceptual rule:
// Best short setups form in premium zone
// Best long setups form in discount zone
```

### 13.4 Projection Calculation - Detailed Formulas

**Body Projections - Bullish Setup:**
```pseudocode
// Reference point: C2 price (low of C2)
reference_point = C2_Price

// HTF body size
body_size = abs(C_HTF - O_HTF)

// For each enabled projection level:
for each projection in enabled_projections:
    multiplier = projection.multiplier  // e.g., -1, -2, -2.5, -4, -4.5
    
    // Bullish setup: Projections extend downward (opposite direction)
    // Negative multiplier means extend in opposite direction
    projection_level = reference_point - (body_size × abs(multiplier))
    
    // Example:
    // C2 = 100, Body = 2, Multiplier = -1
    // Projection = 100 - (2 × 1) = 98
    
    // C2 = 100, Body = 2, Multiplier = -2
    // Projection = 100 - (2 × 2) = 96
    
    plot_projection_line(
        y = projection_level,
        label = multiplier,
        style = projection_line_style,
        width = projection_line_width,
        color = projection_line_color
    )
```

**Body Projections - Bearish Setup:**
```pseudocode
// Reference point: C2 price (high of C2)
reference_point = C2_Price

// HTF body size
body_size = abs(C_HTF - O_HTF)

// For each enabled projection level:
for each projection in enabled_projections:
    multiplier = projection.multiplier  // e.g., -1, -2, -2.5, -4, -4.5
    
    // Bearish setup: Projections extend upward (opposite direction)
    // Negative multiplier means extend in opposite direction
    projection_level = reference_point + (body_size × abs(multiplier))
    
    // Example:
    // C2 = 100, Body = 2, Multiplier = -1
    // Projection = 100 + (2 × 1) = 102
    
    // C2 = 100, Body = 2, Multiplier = -2
    // Projection = 100 + (2 × 2) = 104
    
    plot_projection_line(
        y = projection_level,
        label = multiplier,
        style = projection_line_style,
        width = projection_line_width,
        color = projection_line_color
    )
```

**Wick Projections - Bullish Setup:**
```pseudocode
// Reference point: HTF Low (wick extreme)
reference_point = L_HTF

// HTF range
htf_range = H_HTF - L_HTF

// For each enabled projection level:
for each projection in enabled_projections:
    multiplier = projection.multiplier  // e.g., -1, -2, -2.5, -4, -4.5
    
    // Bullish setup: Projections extend downward from HTF low
    projection_level = reference_point - (htf_range × abs(multiplier))
    
    // Example:
    // HTF Low = 98, Range = 4, Multiplier = -1
    // Projection = 98 - (4 × 1) = 94
    
    plot_projection_line(
        y = projection_level,
        label = "W" + multiplier,  // "W" prefix for wick
        style = projection_line_style,
        width = projection_line_width,
        color = projection_line_color
    )
```

**Wick Projections - Bearish Setup:**
```pseudocode
// Reference point: HTF High (wick extreme)
reference_point = H_HTF

// HTF range
htf_range = H_HTF - L_HTF

// For each enabled projection level:
for each projection in enabled_projections:
    multiplier = projection.multiplier  // e.g., -1, -2, -2.5, -4, -4.5
    
    // Bearish setup: Projections extend upward from HTF high
    projection_level = reference_point + (htf_range × abs(multiplier))
    
    // Example:
    // HTF High = 102, Range = 4, Multiplier = -1
    // Projection = 102 + (4 × 1) = 106
    
    plot_projection_line(
        y = projection_level,
        label = "W" + multiplier,  // "W" prefix for wick
        style = projection_line_style,
        width = projection_line_width,
        color = projection_line_color
    )
```

**Projection Reference Point Selection:**
```pseudocode
// Priority order for reference points:
// 1. C2 (primary reference - always used)
// 2. C3 (secondary - if exists and setup advanced)
// 3. C4 (tertiary - if exists and setup advanced)
// 4. CISD (fallback - if C2 not yet formed)

if C2_exists:
    reference_point = C2_Price
else if CISD_exists:
    reference_point = CISD_Price
else:
    // No valid reference yet, skip projections
    skip_projections = true
```

### 13.5 T-Spot Calculation - Detailed Methodology

**T-Spot Algorithm (General Approach):**
```pseudocode
// T-Spot projects where HTF candle wick is likely to form
// Based on TTrades' methodology combining:
// 1. Previous structure analysis
// 2. Current momentum
// 3. Fibonacci relationships (potentially)
// 4. Orderflow patterns

// For Bullish Setup:
if setup_is_bullish:
    // Analyze previous HTF candle wick formation
    previous_htf_wick_high = previous_H_HTF
    previous_htf_body_top = max(previous_O_HTF, previous_C_HTF)
    previous_htf_wick_size = previous_H_HTF - previous_htf_body_top
    
    // Consider current momentum
    current_momentum = calculate_momentum()  // Based on recent LTF candles
    
    // Project T-Spot above current price
    // Typically: Previous wick high + some extension
    // Or: Current price + (HTF range × extension_factor)
    
    t_spot_level = current_price + (htf_range × t_spot_extension_factor)
    
    // Alternative calculation (if previous structure method):
    // t_spot_level = previous_htf_wick_high + (previous_htf_wick_size × factor)
    
    // Ensure T-Spot is above current price
    if t_spot_level <= current_price:
        t_spot_level = current_price + (htf_range × 0.5)  // Fallback

// For Bearish Setup:
if setup_is_bearish:
    // Similar logic but downward
    previous_htf_wick_low = previous_L_HTF
    previous_htf_body_bottom = min(previous_O_HTF, previous_C_HTF)
    previous_htf_wick_size = previous_htf_body_bottom - previous_L_HTF
    
    current_momentum = calculate_momentum()
    
    t_spot_level = current_price - (htf_range × t_spot_extension_factor)
    
    // Ensure T-Spot is below current price
    if t_spot_level >= current_price:
        t_spot_level = current_price - (htf_range × 0.5)  // Fallback

// Plot T-Spot marker
plot_t_spot_marker(
    y = t_spot_level,
    style = t_spot_marker_style,
    size = t_spot_marker_size,
    color = t_spot_color
)
```

**T-Spot Extension Factor:**
```pseudocode
// Typical extension factors (to be refined):
// Conservative: 0.382 (Fibonacci)
// Moderate: 0.5 (50% of range)
// Aggressive: 0.618 (Fibonacci) or 1.0 (full range)

t_spot_extension_factor = 0.5  // Default, may be adjustable
```

**Note on T-Spot Refinement:**
- Exact T-Spot calculation may require analysis of TTrades' specific methodology
- May involve more complex algorithms considering:
  - Multiple previous HTF candles
  - Volume analysis
  - Orderflow patterns
  - Market structure relationships
- Initial implementation can use simplified approach, then refine based on testing

### 13.6 Formation Liquidity Calculation

**Previous HTF Candles Identification:**
```pseudocode
// Get previous N HTF candles for formation liquidity marking
// These candles provide context for where price may seek liquidity

// Configuration
num_previous_candles = 3  // Typically 2-5 candles, configurable
// More candles = more liquidity zones but more chart clutter
// Fewer candles = cleaner chart but less context

// Initialize storage for previous HTF candles
previous_htf_candles = create_array()

// Request data for previous HTF candles
// Use batched request for efficiency
previous_htf_data = request_multi_timeframe_data(
    symbol = current_symbol,
    timeframe = htf_timeframe,
    data_fields = [HIGH_PRICE, LOW_PRICE],
    lookahead_mode = DISABLED,
    bar_offsets = [1, 2, 3]  // Previous 3 HTF candles
)

// Process and store previous HTF candle data
for i = 0 to num_previous_candles - 1:
    previous_candle = {
        bar_offset: i + 1,  // 1 = most recent previous, 2 = second previous, etc.
        high: previous_htf_data[i * 2],      // High price
        low: previous_htf_data[i * 2 + 1],   // Low price
        range: previous_htf_data[i * 2] - previous_htf_data[i * 2 + 1],
        midpoint: (previous_htf_data[i * 2] + previous_htf_data[i * 2 + 1]) / 2,
        timestamp: get_htf_candle_timestamp(htf_timeframe, current_time - (i + 1) * htf_period)
    }
    
    add_to_array(previous_htf_candles, previous_candle)

// Validate data availability
for each candle in previous_htf_candles:
    if candle.high == NULL OR candle.low == NULL:
        show_warning("Insufficient historical HTF data for liquidity marking")
        // Reduce number of candles to mark
        num_previous_candles = get_valid_candle_count(previous_htf_candles)
        break
```

**Liquidity Zone Marking:**
```pseudocode
// For Bullish Setup:
// Mark previous HTF highs as liquidity above (resistance)
for each previous_htf_high:
    plot_liquidity_marker(
        y = previous_htf_high,
        type = "resistance",
        style = formation_liquidity_line_style,
        width = formation_liquidity_line_width,
        color = formation_liquidity_color
    )

// For Bearish Setup:
// Mark previous HTF lows as liquidity below (support)
for each previous_htf_low:
    plot_liquidity_marker(
        y = previous_htf_low,
        type = "support",
        style = formation_liquidity_line_style,
        width = formation_liquidity_line_width,
        color = formation_liquidity_color
    )
```

### 13.7 Candle 1 Liquidity Sweep Calculation

**Candle 1 Identification:**
```pseudocode
// Candle 1 is the initial swing point in the formation
// For Bullish: First significant low (C2 or CISD low, whichever is first)
// For Bearish: First significant high (C2 or CISD high, whichever is first)

if setup_is_bullish:
    candle_1_level = min(CISD_Low, C2_Low)
    candle_1_type = "support"
else:
    candle_1_level = max(CISD_High, C2_High)
    candle_1_type = "resistance"
```

**Liquidity Sweep Marker:**
```pseudocode
// Draw horizontal ray marking Candle 1 level
plot_candle_1_sweep(
    y = candle_1_level,
    style = candle_1_sweep_line_style,
    width = candle_1_sweep_line_width,
    color = candle_1_sweep_color,
    extend = extend_right  // Ray extends to right
)
```

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
