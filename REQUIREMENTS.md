# Fractal Model Pro (TTrades) - Requirements Document

## 1. Executive Summary

This document outlines the requirements for developing a Pinescript indicator that replicates the **Fractal Model Pro (TTrades)** indicator. The indicator is designed to identify Algorithmic Price Delivery patterns by analyzing price movements across multiple timeframes, detecting momentum shifts, swing formations, and orderflow continuations.

**Key Characteristics:**
- Non-repainting and stable within the given time period
- Multi-timeframe analysis (Higher Timeframe / Lower Timeframe pairing)
- Automated detection of price delivery patterns
- Customizable visualization and alert system
- **Complete automation:** Automates the entire fractal model sequence, highlights candle rates, plots the change in state of delivery, and overlays higher timeframe context directly onto lower timeframe charts
- **Structured tool:** One of the most structured tools for understanding algorithmic delivery
- **Mechanical structure:** Brings mechanical structure, clarity, and consistency to charting process

**Document Structure:**
This requirements document is designed to be reviewed and understood by both technical and non-technical stakeholders. All algorithm flows, calculation processes, and system behaviors are explained in plain English with step-by-step procedures. Technical implementation details are presented as clear, narrative explanations that describe "what happens" and "how it works" rather than "how to code it." This ensures that the customer can fully understand the indicator's mechanics and verify that all requirements are met.

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
- **Range:** 1 to 40
- **Default:** 4 (or as specified)
- **Purpose:** Controls how many HTF candles are plotted on chart
- **Behavior:** 
  - Lower values: Focus on recent structure
  - Higher values: More historical context and extended visibility of swing development
  - When changed, the indicator plots additional HTF candles and marks vertical lines on the chart
  - Example: Setting to 4 displays 4 HTF candles; changing to 8 plots 4 additional candles
- **Impact:** 
  - Affects chart clarity vs. historical depth
  - Provides equilibrium tracking and cleaner framework recognition
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

#### 2.3.12 Show Early CISD (Early C2 CISD)
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Highlight early CISD within C2 candle for earlier detection
- **Behavior:** 
  - Plots the CISD before the candle has closed, making sure you can see the developing change in state of delivery
  - Shows potential CISD before full confirmation
  - Also referred to as "Early C2 CISD" in documentation
- **Impact:** Earlier setup identification, may have lower reliability, but provides earlier warning of potential setup formation

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
- **Type:** String (symbol input) or Symbol Selector
- **Default:** ""
- **Purpose:** First market symbol for manual SMT pair
- **Example:** "CME_MINI:ES1!" (E-mini S&P 500)
- **Selection Method:** User can select the asset from the "Change Symbol" tab/interface
- **Validation:** Must be valid TradingView symbol
- **Impact:** Only used when SMT Mode = "Manual"

#### 2.6.4 SMT Pair 2 (Manual Mode)
- **Type:** String (symbol input) or Symbol Selector
- **Default:** ""
- **Purpose:** Second market symbol for manual SMT pair
- **Example:** "CME_MINI:YM1!" (E-mini Dow)
- **Selection Method:** User can select the asset from the "Change Symbol" tab/interface
- **Note:** It's possible to add and configure a second SMT pair for additional correlation analysis
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

### 2.1.1 The Four-Candle Sequence - Core Algorithm

At the core of the Fractal Model is the idea that **every reversal begins with a three-candle sequence** (extended to four candles for continuation), and each candle has a specific role in the algorithmic delivery process. This behavior is fractal, meaning the same candle process occurs on every timeframe from higher timeframe swings to intraday execution.

**Candle 1 (C1) - Sets the Liquidity:**
- **Role:** Establishes the engineered liquidity pool
- **Function:** Forms the high or low that becomes the target for the next candle to trade
- **Mechanics:** This candle creates the initial swing point (high for bearish setups, low for bullish setups)
- **Purpose:** Sets up the liquidity level that Candle 2 will target and "sweep"
- **Visual:** Marked with a horizontal ray showing the liquidity level

**Candle 2 (C2) - Performs the Sweep and Produces CISD:**
- **Role:** Executes the liquidity sweep and confirms the change in state of delivery
- **Function:** 
  - Takes out Candle 1's liquidity (sweeps the high or low established by C1)
  - Closes back inside the previous range
  - Produces the CISD (Change in State of Delivery)
- **Mechanics:** This shift in delivery confirms that a reversal is unfolding
- **Purpose:** Validates that the reversal sequence has begun
- **Visual:** Labeled as "C2" and marks the CISD point

**Candle 3 (C3) - The Expansion Phase:**
- **Role:** Initiates the expansion move away from the reversal point
- **Function:**
  - Expands away from the CISD
  - Targets any remaining liquidity left inside Candle 2 and Candle 1
- **Mechanics:** This is where the new trend direction gains momentum
- **Purpose:** Confirms the reversal and begins the expansion phase
- **Visual:** Labeled as "C3" and shows the expansion direction

**Candle 4 (C4) - The Continuation:**
- **Role:** Continues the expansion move initiated in Candle 3
- **Function:** Looks to continue the expansion move, often targeting projection levels
- **Mechanics:** Extends the trend established in C3
- **Purpose:** Shows continuation of the new orderflow direction
- **Visual:** Labeled as "C4" and indicates continuation

**Sequence Development:**
- The Fractal Model automatically labels the structure as C1, C2, C3, and C4 as the sequence develops
- Not all setups will have all four candles - some may only develop C1, C2, and C3
- The sequence can occur on any timeframe, making it truly fractal in nature
- Higher timeframe swings follow the same pattern as lower timeframe execution

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
  - 1m-5m, 3m-30m, 5m-15m, 5m-1H, 15m-1H, 15m-4H, 1H-4H, 1H-1D, 4H-1D, 1D-1W
  - These represent typical fractal relationships where HTF is 3-12x the LTF
  - Examples from documentation: 3m-30m (10x ratio), 5m-1H (12x ratio), 1H-1D (24x ratio)
- **Custom pairings:** User-defined HTF-LTF combinations
  - Any valid timeframe combination where HTF > LTF
  - Examples: 1H-3m, 1D-15m (allows flexibility for personal framework preferences)
  - Must respect TradingView timeframe hierarchy
- **Automatic pairing:** System automatically selects appropriate HTF based on current chart timeframe
  - Uses predefined rules to select optimal HTF
  - Ensures proper fractal relationship
  - Can be overridden with custom pairing if desired

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
- **Lookback History**: Control how many HTF candles are plotted (range: 1 to 40)
  - Increasing the value provides more historical structure and broader view of swing development
  - Lower values keep focus on the most recent expansion
  - When changed, the indicator plots additional HTF candles and marks vertical lines on the chart

**Timestamp Labels:**
- **Time Labels**: Option to display exact timestamps showing when each HTF candle formed
- When enabled, the indicator plots the exact times at which the HTF candles formed onto the chart
- Provides additional context in relation to time, helping traders understand when key structural points occurred
- Example: If viewing 1H HTF candles, labels show "10:00", "11:00", "12:00", etc.

**Vertical Lines:**
- When HTF candle lookback history is increased, the indicator automatically marks vertical lines on the chart
- These vertical lines correspond to the HTF candle boundaries
- Helps visualize the HTF structure and align it with LTF price action
- Example: If displaying 8 HTF candles, 8 vertical lines mark the boundaries of each HTF candle period

**Previous Candle EQ:**
- Display equilibrium (50% level) of the previous HTF candle
- Can be viewed on both HTF candle and chart directly

### 3.2 Change in State of Delivery (CISD)

**Purpose:** Identify the first potential change in orderflow, signaling a shift from sell program to buy program or vice versa.

**Mechanics:**
- Marks the series of candles making up significant highs or lows
- **CISD Confirmation:** A close beyond the opening price signals a change from bullish to bearish (or vice versa)
- This confirms a trend reversal and is a form of orderblock
- **Early C2 CISD (NEW):** Option to highlight early CISD within the C2 candle for earlier detection
  - Plots the CISD before the candle has closed, allowing you to see the developing change in state of delivery
  - Provides earlier warning of potential setup formation
  - Becomes confirmed CISD when the candle closes and full criteria are met

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

**Purpose:** Highlight important liquidity levels at each swing point. Candle 1 sets the liquidity and forms the high or low that becomes the target for the next candle to trade. It's the engineered liquidity pool.

**Mechanics:**
- **Candle 1's Role:** Sets the liquidity that Candle 2 will target and sweep
- Forms the high (for bearish setups) or low (for bullish setups) that becomes the target
- This is the engineered liquidity pool - the level where stops are likely located
- Horizontal rays mark sweeps of liquidity at the C1 level
- Identifies potential reversal points
- Marks the initial liquidity marker in the formation
- **Connection to Formation Liquidity:** Formation liquidity is set at the lower candle at C1, from the swing point

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

### 3.7 Projections (Standard Deviation Projections)

**Purpose:** Leverage projected levels based on shifts in delivery for future price redelivery, rebalancing, and exhaustion points. These are also referred to as "standard deviation projections" in the documentation.

**Mechanics:**
- User-defined projection levels based on past orderblock behavior
- Default projections: [-1, -2, -2.5, -4, -4.5]
- **Dynamic Addition:** Users can add multiple additional projection levels
  - Example: Adding -1.5 standard deviation projection
  - The indicator uploads/plots the new projection level (e.g., minus one and a half standard deviation)
  - Allows customization beyond the default five levels
- Two types of projections:
  - **Body Projections:** Based on HTF candle body
  - **Wick Projections:** Based on HTF candle wicks

**Calculation Logic:**
- Projections extend from key reference points (likely C2, C3, C4 or CISD points)
- Multipliers determine distance from reference (e.g., -1 = 1x range, -2 = 2x range)
- Negative values suggest extension in opposite direction of trend
- Projections can target order block levels (e.g., second standard deviation projection from the order block)

**Customization:**
- Add or remove projection levels dynamically
- Enable/disable projection display
- Label visibility and styling
- Label size, style, and color customization

### 3.8 Formation Liquidity

**Purpose:** Identify previous candles' highs and lows as critical liquidity points for the current developing formation.

**Mechanics:**
- **Formation liquidity is basically liquidity that's set at the lower candle at C1** (Candle 1)
- Marks engineered liquidity pools from the swing point at C1
- Highlights previous swing highs and lows that relate to the current formation
- For active setups, the formation liquidity shows liquidity from the swing point at C1
- Example: In an active bearish model, the formation liquidity is from the swing point at C1
- Serves as key reference points for future price action
- Helps identify areas where price may seek liquidity

**Customization:**
- Toggle to show/hide liquidity markers
- Line style (dotted/solid)
- Line width
- Color options

---

## 3.9 Practical Examples - How the Fractal Model Works in Real Trading

### Example 1: Higher Timeframe Bearish Setup

**Scenario:** A swing forms on the hourly (1H) timeframe, and the fractal model identifies the sequence:

1. **Candle 1 (C1):** Sets the initial liquidity
   - Forms a high that becomes the target for the next candle
   - This is the engineered liquidity pool

2. **Candle 2 (C2):** Performs the sweep and produces CISD
   - Takes out Candle 1's liquidity (sweeps the high)
   - Closes back inside the previous range
   - Produces the Change in State of Delivery (CISD)
   - This confirms the reversal is unfolding

3. **Candle 3 (C3):** The expansion phase
   - Expands away from the CISD
   - Continues the orderflow lower (in this bearish example)
   - Targets any remaining liquidity left inside Candle 2 and Candle 1

**Result:** The model successfully identifies a bearish reversal and expansion on the hourly timeframe, providing clear structure for lower timeframe execution.

### Example 2: Lower Timeframe Bullish Setup (EUR/USD)

**Scenario:** A bullish framework on EUR/USD (Euro vs. US Dollar) on a lower timeframe:

1. **Candle 1 (C1):** Sets the initial liquidity
   - Forms the initial low here
   - Establishes the liquidity pool that will be targeted

2. **Candle 2 (C2):** Rates the liquidity pool and produces CISD
   - Takes out Candle 1's liquidity (sweeps the low)
   - Closes back inside the range
   - Produces the CISD (change from bearish to bullish delivery)

3. **Candle 3 (C3):** Expands away from CISD
   - Expands upward, confirming the bullish reversal
   - Begins the new bullish orderflow

4. **Candle 4 (C4):** Produces continuation
   - Continues the expansion move
   - Extends all the way until reaching the second standard deviation projection from the order block
   - Shows the continuation of the bullish move

**Result:** The complete four-candle sequence is identified, with price reaching projection targets, demonstrating how the fractal model tracks the full algorithmic delivery sequence.

### Key Takeaways from Examples:

- **Fractal Nature:** The same candle sequence process occurs on every timeframe
- **Structure Recognition:** The model automatically labels C1, C2, C3, C4 as the sequence develops
- **Projection Targeting:** Price often reaches projection levels (e.g., second standard deviation projection)
- **Orderflow Continuation:** The model tracks how orderflow continues through the sequence
- **Multi-Timeframe:** Higher timeframe swings follow the same pattern as lower timeframe execution

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

**Failure Detection Process - Step by Step:**

**For Bullish Setups:**

1. **Identify the Initial Low Point:**
   - Look at both the CISD low price and the C2 low price
   - Take whichever is lower - this becomes the "Initial Low"
   - This is the lowest point where the bullish setup began

2. **Monitor Current Price Action:**
   - Track the lowest price that has occurred since the setup was formed
   - This is called the "Current Price Low"

3. **Check if Price Returned to Initial Low:**
   - Compare: Has the Current Price Low gone back down to or below the Initial Low?
   - If NO → Setup is still valid, continue monitoring
   - If YES → Proceed to step 4

4. **Check Higher Timeframe Structure:**
   - Look at the previous Higher Timeframe candle's low price
   - Look at the current Higher Timeframe candle's low price
   - Compare: Is the current HTF low LOWER than the previous HTF low?
   - If YES → A new HTF swing low has formed (structure maintained at higher level)
   - If NO → No new HTF swing low formed

5. **Determine Setup Failure:**
   - If price returned to Initial Low (step 3) AND no HTF swing low formed (step 4):
     - **Setup has FAILED**
     - Change all labels (C2, C3, C4) to RED color
     - Stop displaying all projections, equilibrium lines, liquidity sweeps, and T-spots
     - Keep the labels visible but in red (for learning purposes)
   - If price returned BUT an HTF swing low DID form:
     - Setup may still be valid (structure maintained at higher timeframe)
     - Continue monitoring

**For Bearish Setups:**

1. **Identify the Initial High Point:**
   - Look at both the CISD high price and the C2 high price
   - Take whichever is higher - this becomes the "Initial High"
   - This is the highest point where the bearish setup began

2. **Monitor Current Price Action:**
   - Track the highest price that has occurred since the setup was formed
   - This is called the "Current Price High"

3. **Check if Price Returned to Initial High:**
   - Compare: Has the Current Price High gone back up to or above the Initial High?
   - If NO → Setup is still valid, continue monitoring
   - If YES → Proceed to step 4

4. **Check Higher Timeframe Structure:**
   - Look at the previous Higher Timeframe candle's high price
   - Look at the current Higher Timeframe candle's high price
   - Compare: Is the current HTF high HIGHER than the previous HTF high?
   - If YES → A new HTF swing high has formed (structure maintained at higher level)
   - If NO → No new HTF swing high formed

5. **Determine Setup Failure:**
   - If price returned to Initial High (step 3) AND no HTF swing high formed (step 4):
     - **Setup has FAILED**
     - Change all labels (C2, C3, C4) to RED color
     - Stop displaying all projections, equilibrium lines, liquidity sweeps, and T-spots
     - Keep the labels visible but in red (for learning purposes)
   - If price returned BUT an HTF swing high DID form:
     - Setup may still be valid (structure maintained at higher timeframe)
     - Continue monitoring

**Key Points:**
- Failure requires BOTH conditions: Price return AND no HTF swing formation
- If HTF swing forms, setup may still be valid (structure maintained at higher level)
- Failure is permanent for that setup (does not recover)
- Failed setups remain visible (red labels) for learning/reference

### 4.3 Consolidation Detection - Detailed

**Consolidation Detection Process - Step by Step:**

**For Each Active Setup (with Gray Labels):**

1. **Wait for Setup Formation Period to Complete:**
   - The setup needs time to develop - wait for the Higher Timeframe candle to close
   - This is the candle period during which the setup was forming
   - Once this HTF candle closes, we can evaluate if consolidation occurred

2. **For Bullish Setups - Check Progress:**
   - **Progress Check:** Has the price made significant new highs since the setup formed?
     - Look at the highest price reached since setup formation
     - Compare to the setup's initial high point
     - If price made substantially higher highs → Progress is good
     - If price hasn't made new highs → No progress
   
   - **Range Check:** Is the price moving sideways in a range?
     - Look at recent price action
     - Is price bouncing between similar high and low levels?
     - If yes → Price is range-bound
     - If no → Price is trending

3. **For Bearish Setups - Check Progress:**
   - **Progress Check:** Has the price made significant new lows since the setup formed?
     - Look at the lowest price reached since setup formation
     - Compare to the setup's initial low point
     - If price made substantially lower lows → Progress is good
     - If price hasn't made new lows → No progress
   
   - **Range Check:** Is the price moving sideways in a range?
     - Look at recent price action
     - Is price bouncing between similar high and low levels?
     - If yes → Price is range-bound
     - If no → Price is trending

4. **Determine Consolidation:**
   - If BOTH conditions are true:
     - NO significant progress (price hasn't moved in expected direction)
     - AND price is moving in a range (sideways movement)
   - Then: **Consolidation is Detected**

5. **Actions When Consolidation Detected:**
   - Change all labels (C2, C3, C4) to ORANGE color
   - **Keep all projections active** (unlike failure where they stop)
   - **Setup remains valid** (unlike failure where it's invalidated)
   - Orange color signals: "Setup is still good, but momentum has paused"

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

**Purpose:** The autobias module adds directional filtering to the fractal model, ensuring the model only activates sequences that align with the dominant higher timeframe direction. It removes counter-trend setups that plot, and bias updates in real-time as each higher timeframe candle closes.

**Auto Bias 1:**
- References one timeframe higher than the current chart to determine structural bias
- Aligns fractal models with one timeframe higher than current chart
- Automatic bias selection based on HTF context
- Example: If viewing a 5-minute chart, Auto Bias 1 uses the 15-minute or 1-hour timeframe to determine bias

**Auto Bias 2:**
- References two timeframes higher than the current chart to determine structural bias
- Aligns fractal models with two timeframes higher than current chart
- Creates an even stricter filtering system
- Provides even higher timeframe context for more conservative filtering

**How It Works:**
- When autobias is enabled, the system checks the higher timeframe structure
- Only setups that align with the higher timeframe direction are displayed
- Example: If you have a 1-hour higher timeframe bullish model, only bullish models will print in the lower timeframe price action
- Bearish models get filtered out automatically
- This ensures you only see setups that align with the dominant trend

**Real-Time Updates:**
- Bias updates in real-time as each higher timeframe candle closes
- As the higher timeframe structure changes, the bias filter automatically adjusts
- This keeps the indicator aligned with current market structure

**Manual Bias:**
- User can manually set bias independent of timeframe (Neutral, Bullish, or Bearish)
- Manual bias overrides autobias when selected

---

## 6. Time Filtering System

### 6.1 Time Filter Threshold - Detailed Logic

**Purpose:** Specify a timeframe threshold below which time filters will be applied. This prevents time filters from being applied on higher timeframes where they become less relevant.

**How Time Filter Threshold Works - Step by Step:**

1. **Convert Timeframes to Minutes:**
   - Take the current chart timeframe (e.g., 5 minutes, 1 hour, 4 hours)
   - Convert it to total minutes:
     - 1 minute = 1 minute
     - 5 minutes = 5 minutes
     - 15 minutes = 15 minutes
     - 1 hour = 60 minutes
     - 4 hours = 240 minutes
     - 1 day = 1,440 minutes
   
   - Take the threshold timeframe setting (e.g., "1H")
   - Convert it to total minutes (e.g., 1H = 60 minutes)

2. **Compare the Two Timeframes:**
   - Compare: Is the current chart timeframe LESS THAN OR EQUAL TO the threshold?
   - If YES → Time filters are ACTIVE (they will be applied)
   - If NO → Time filters are IGNORED (all setups shown regardless of time)

3. **Example Scenarios:**

   **Scenario 1: 5-Minute Chart with 1-Hour Threshold**
   - Chart timeframe: 5 minutes
   - Threshold: 1 hour (60 minutes)
   - Comparison: 5 ≤ 60? YES
   - Result: Time filters are ACTIVE ✓
   - Meaning: Setups will only show during your specified time windows

   **Scenario 2: 1-Hour Chart with 1-Hour Threshold**
   - Chart timeframe: 1 hour (60 minutes)
   - Threshold: 1 hour (60 minutes)
   - Comparison: 60 ≤ 60? YES
   - Result: Time filters are ACTIVE ✓
   - Meaning: Setups will only show during your specified time windows

   **Scenario 3: 4-Hour Chart with 1-Hour Threshold**
   - Chart timeframe: 4 hours (240 minutes)
   - Threshold: 1 hour (60 minutes)
   - Comparison: 240 ≤ 60? NO
   - Result: Time filters are IGNORED ✗
   - Meaning: All setups will show regardless of time of day
   - Reason: On higher timeframes, time of day becomes less relevant than market structure

**Rationale:**
- Time filters are most useful on lower timeframes (1m, 5m, 15m)
- On higher timeframes (4H, 1D), time of day becomes less relevant
- Market structure and HTF context matter more than specific hours on higher timeframes
- Prevents unnecessary filtering on timeframes where it doesn't add value

### 6.2 Custom Time Filters - Detailed Mechanics

**Purpose:** Filter setup detection to specific time windows, allowing traders to focus on their preferred trading sessions (e.g., London open, New York open, Asian session).

**Practical Example:**
- If you only want to see setups between 8:00 AM and 11:00 AM New York time:
  1. Enable Time Filter 1
  2. Set Start Time to 08:00
  3. Set End Time to 11:00
  4. Set Custom Timezone to -5 (Eastern Time) or adjust for your timezone
  5. The indicator will only show setups that occur during this time window
  6. All setups outside this window will be filtered out

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

**How Time Filters Are Applied - Step by Step:**

**When a New Setup is Detected:**

1. **Check if Time Filters Are Active:**
   - First, check the threshold setting (see previous section)
   - If time filters are NOT active (chart timeframe is too high):
     - Show the setup regardless of time
     - Skip the rest of this process
   - If time filters ARE active:
     - Continue to step 2

2. **Get Current Time in Your Timezone:**
   - The system gets the current time in UTC (Coordinated Universal Time)
   - It then converts this to your custom timezone setting
   - For example: If it's 10:00 AM UTC and your timezone is -5 hours (Eastern Time):
     - Your local time = 10:00 - 5 hours = 5:00 AM
   - Extract just the hour and minute (e.g., "05:00")

3. **Check Each Enabled Time Filter:**
   - The system checks if the current time falls within any of your enabled time windows
   - It checks Time Filter 1, then Time Filter 2, then Time Filter 3
   - If the current time falls within ANY enabled filter → Setup is valid
   - If the current time does NOT fall within any enabled filter → Setup is hidden

4. **Normal Time Window (Start Time < End Time):**
   - Example: Time Filter 1 is set to 02:00 to 05:00
   - The window includes the start time but excludes the end time
   - **01:59** → Before window, setup NOT shown
   - **02:00** → Start of window, setup IS shown ✓
   - **03:30** → Within window, setup IS shown ✓
   - **04:59** → Within window, setup IS shown ✓
   - **05:00** → End of window (excluded), setup NOT shown
   - **05:01** → After window, setup NOT shown

5. **Wrap-Around Time Window (Crosses Midnight):**
   - Example: Time Filter is set to 22:00 to 02:00 (overnight)
   - This window crosses midnight, so it includes late evening and early morning
   - **21:59** → Before window, setup NOT shown
   - **22:00** → Start of window, setup IS shown ✓
   - **23:30** → Within window, setup IS shown ✓
   - **00:00** → After midnight, still within window, setup IS shown ✓
   - **01:59** → Still within window, setup IS shown ✓
   - **02:00** → End of window (excluded), setup NOT shown
   - **02:01** → After window, setup NOT shown

6. **Multiple Time Filters:**
   - If you have multiple time filters enabled (e.g., Filter 1 AND Filter 2):
   - The setup is shown if it falls within ANY of the enabled filters
   - Example:
     - Filter 1: 02:00-05:00 (enabled)
     - Filter 2: 08:00-11:00 (enabled)
     - Filter 3: 14:00-17:00 (disabled)
   - Setup at 03:00 → Shown (within Filter 1) ✓
   - Setup at 09:30 → Shown (within Filter 2) ✓
   - Setup at 15:00 → NOT shown (Filter 3 disabled, not in Filter 1 or 2)
   - Setup at 06:00 → NOT shown (not in any enabled filter)

7. **No Filters Enabled:**
   - If none of the three time filters are enabled:
   - All setups are shown regardless of time

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

**Key Characteristics:**
- **SMT highlights when one asset makes a lower low while the correlated asset makes a higher high** (or vice versa)
- This creates a divergence that often **marks exhaustion in delivery**
- **SMT is NOT an entry trigger** - it is a **confirmation tool**
- **Typical scenario example:** NASDAQ creating a higher high while ES (S&P 500) is creating a lower high - they're not moving in the same direction, thus creating SMT divergence
- **When SMT aligns with a validated CISD, it strengthens the narrative and improves timing**
- Provides multi-market confirmation to validate fractal model setups

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
**SMT Divergence Detection Process - Step by Step:**

1. **Get Price Data for Both Markets:**
   - The system requests price data for Pair 1 (the charted symbol or related market)
   - The system requests price data for Pair 2 (the correlated market)
   - Both use the same timeframe as your current chart
   - The system looks back at the last 50-100 price bars for analysis
   - It collects: closing prices, high prices, and low prices for both markets

2. **Validate Data Availability:**
   - Check if data is available for both Pair 1 and Pair 2
   - If data is missing for either market:
     - Show a warning message: "SMT data unavailable"
     - Disable the SMT divergence feature
     - Stop the process here
   - If data is available for both:
     - Continue to step 3

3. **Handle Inverse Correlation (if enabled):**
   - Inverse correlation means: When Pair 1 goes up, Pair 2 goes down (and vice versa)
   - This only applies if you're using Manual mode (Auto mode already knows the correlation type)
   - If inverse correlation is enabled:
     - The system finds a "base price" for Pair 2 (like an average or starting value)
     - For each price point in Pair 2:
       - Calculate how far it is from the base price
       - Flip it to the opposite side of the base price
       - Example: If price is 2 points above base, flip it to 2 points below base
     - This allows the system to compare markets that move in opposite directions

4. **Calculate Trends for Both Markets:**
   - **For Pair 1:**
     - Look at the most recent period (e.g., last 20 bars)
     - Find the highest price and lowest price in that period
     - Look at the more recent half of that period (e.g., last 10 bars)
     - Find the highest and lowest in that recent half
     - Compare:
       - If recent highs are higher AND recent lows are higher → Trend is "Bullish"
       - If recent highs are lower AND recent lows are lower → Trend is "Bearish"
       - Otherwise → Trend is "Neutral"
   
   - **For Pair 2:**
     - Do the same analysis as Pair 1
     - Determine if trend is Bullish, Bearish, or Neutral

5. **Detect Divergence:**
   - **Bullish Divergence:**
     - Pair 1 is making lower lows (Bearish trend)
     - BUT Pair 2 is making higher lows (Bullish trend)
     - This means: Pair 1 is weakening, but Pair 2 is strengthening
     - This can signal a potential reversal upward in Pair 1
   
   - **Bearish Divergence:**
     - Pair 1 is making higher highs (Bullish trend)
     - BUT Pair 2 is making lower highs (Bearish trend)
     - This means: Pair 1 is strengthening, but Pair 2 is weakening
     - This can signal a potential reversal downward in Pair 1
   
   - **No Divergence:**
     - Both markets are moving in the same direction
     - Or both are neutral
     - No divergence detected

6. **Display Divergence on Chart:**
   - If divergence is detected:
     - Draw a line on your chart showing the divergence
     - The line connects:
       - Start point: Where the divergence began (earlier bar)
       - End point: Current bar (where divergence is confirmed)
     - Use the color, style, and width you specified in settings
   
   - If labels are enabled:
     - Display a text label showing the divergence type
     - Text will say either "Bullish Divergence" or "Bearish Divergence"
     - Place it at the current price level
     - Use the label size and color you specified

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
   - The system displays the current Higher Timeframe and Lower Timeframe relationship
   - Format: "Fractal: [LTF] / [HTF]"
   - Example: "Fractal: 5m / 1H" means you're analyzing 5-minute structure within 1-hour candles
   - If you're using Automatic mode, it adds "(Auto)" to show the pairing was selected automatically
   - Example with Auto: "Fractal: 5m / 1H (Auto)"

**2. Time Until Next HTF Candle Close:**
   - The system calculates how much time remains until the current Higher Timeframe candle closes
   - Process:
     1. Get the current time (right now)
     2. Calculate when the current HTF candle period will end
     3. Subtract current time from end time to get time remaining
     4. Convert the time remaining into hours, minutes, and seconds
     5. Display in format: "Time to Close: [X]h [Y]m [Z]s"
   - Example: "Time to Close: 0h 23m 45s" means 23 minutes and 45 seconds until the HTF candle closes
   - This updates in real-time, counting down as time passes
   - Useful for timing entries/exits relative to HTF structure

**3. Current Bias:**
   - The system displays your current bias setting
   - Shows exactly what you selected in the Bias Selection dropdown
   - Examples:
     - "Bias: Neutral" - Showing both bullish and bearish setups
     - "Bias: Bullish" - Only showing bullish setups
     - "Bias: Bearish" - Only showing bearish setups
     - "Bias: Auto Bias 1" - Bias automatically determined from one timeframe higher
     - "Bias: Auto Bias 2" - Bias automatically determined from two timeframes higher

**4. Time Filter Status:**
   - The system shows which time filters are currently active
   - Process:
     1. Start with text: "Time Filters: "
     2. If Time Filter 1 is enabled, add its window: "[02:00-05:00] "
     3. If Time Filter 2 is enabled, add its window: "[08:00-11:00] "
     4. If Time Filter 3 is enabled, add its window: "[14:00-17:00] "
     5. If no filters are enabled, show: "None"
   - Example with multiple filters: "Time Filters: [02:00-05:00] [08:00-11:00]"
   - Example with no filters: "Time Filters: None"
   - This helps you quickly see which time windows are active

**5. Warning Messages:**
   - The system displays warnings if there are issues with your settings
   - **Warning 1 - LTF Analysis Unavailable:**
     - Triggered when: Your current chart timeframe is higher than the Lower Timeframe
     - Example: Viewing a 15-minute chart when using 5m-1H pairing
     - Message: "⚠ LTF analysis unavailable"
     - Color: Orange (warning, not error)
     - Meaning: HTF candles still work, but LTF features (CISD, LTF projections) won't display
   
   - **Warning 2 - Invalid Timeframe Pairing:**
     - Triggered when: Higher Timeframe is not greater than Lower Timeframe
     - Example: Trying to use 5m as HTF and 15m as LTF (backwards)
     - Message: "⚠ Invalid timeframe pairing"
     - Color: Red (error)
     - Meaning: The indicator cannot function with this pairing

**6. Active Setup Count:**
   - The system counts how many setups are currently active on your chart
   - This includes all setups within your History Setups setting (0 to 40)
   - Format: "Active Setups: [number]"
   - Example: "Active Setups: 3" means there are 3 setups currently displayed
   - This helps you see how many setups the indicator is tracking

**How the Info Table is Built and Displayed:**

**Table Structure:**
1. **Collect Information Items:**
   - The system gathers all the information pieces described above:
     - Fractal Pairing information
     - Time to Close countdown
     - Current Bias setting
     - Time Filter status
     - Active Setup count
   - Each item has two parts: a label (like "Fractal Pairing") and a value (like "5m / 1H")
   - If there are any warnings, those are added to the list as well

2. **Create the Table:**
   - The system creates a table with 2 columns:
     - Column 1: Information labels (left side)
     - Column 2: Information values (right side)
   - Number of rows equals the number of information items you've enabled
   - Table background color uses your setting
   - If borders are enabled, grid lines separate each cell
   - If borders are disabled, it's a clean look without lines

3. **Position the Table:**
   - The system places the table based on your position setting:
     - **Top Left:** Upper left corner of chart
     - **Top Right:** Upper right corner of chart
     - **Top Center:** Top center of chart
     - **Bottom Left:** Lower left corner of chart
     - **Bottom Right:** Lower right corner of chart
     - **Bottom Center:** Bottom center of chart
     - **On Chart:** Displayed directly on the price chart (see alternative below)

4. **Fill in the Table:**
   - For each row in the table:
     - Left cell shows the information label (e.g., "Fractal Pairing")
     - Right cell shows the information value (e.g., "5m / 1H")
     - Uses the colors and sizes you specified in settings

**Alternative: On-Chart Display:**
   - If you select "On Chart" as the position:
     - Instead of a table in a corner, information is displayed directly on the price chart
     - The system calculates a position below the Higher Timeframe candles
     - Each information item is shown as a text label
     - Format: "[Label]: [Value]" (e.g., "Fractal Pairing: 5m / 1H")
     - Labels are stacked vertically, one below the other
     - Uses the text size and color you specified
   - This keeps the information visible while viewing price action

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

**1. How Higher Timeframe Data is Requested (Non-Repainting):**

**The Problem of Repainting:**
- Repainting occurs when an indicator uses information that wasn't available when a bar was forming
- For example: Using data from a bar that hasn't closed yet to calculate something on a previous bar
- This causes historical values to change as new bars form, making the indicator unreliable
- You might see a setup appear, then disappear, or projections that shift around

**The Solution - Non-Repainting Data Requests:**
1. **Request Only Confirmed Data:**
   - When requesting Higher Timeframe data, the system must ONLY use candles that have already closed
   - It cannot use data from a HTF candle that is still forming
   - This ensures that once a bar is in the past, its data never changes

2. **What Data is Requested:**
   - For each Higher Timeframe candle, the system requests:
     - Open price (where the candle started)
     - High price (highest point reached)
     - Low price (lowest point reached)
     - Close price (where the candle ended)
     - Volume (optional, if needed)

3. **Critical Setting - Lookahead Mode:**
   - The system must be configured with "Lookahead Mode = DISABLED"
   - This means: "Do not look ahead to future data"
   - When DISABLED:
     - Only confirmed (closed) HTF candles are used
     - Historical values remain stable and never change
     - No forward-looking bias
     - The indicator is reliable for backtesting

4. **What Happens if Lookahead is Enabled (WRONG):**
   - Historical setups could change as new bars form
   - Projections could shift around
   - Labels could move to different positions
   - The indicator would be unreliable
   - You couldn't trust backtesting results
   - **This must NEVER happen**

**2. How the System Handles Different Bar States:**

**Understanding Bar States:**
The indicator must distinguish between two types of bars:

1. **Confirmed (Closed) Bars:**
   - These are bars that have finished forming
   - They are in the past and will never change
   - Their open, high, low, and close prices are final
   - These are safe to use for permanent calculations

2. **Unconfirmed (Forming) Bars:**
   - This is the current bar that is still forming
   - It updates in real-time as new price data comes in
   - Its prices can change until it closes
   - This is the "live" bar you see updating

**How the System Processes Each Bar:**

**For Confirmed (Closed) Bars:**
1. The system detects that a bar has closed
2. This bar's data is now final and will never change
3. The system can safely:
   - Detect new setups based on this confirmed data
   - Calculate projection levels (these will be permanent)
   - Update labels (positions are now fixed)
   - Store setup data permanently in memory
4. All calculations based on this bar are final

**For the Current (Forming) Bar:**
1. The system detects this is the bar currently forming
2. This bar's data may still change as price moves
3. The system can show:
   - Real-time Higher Timeframe candle development (as it forms)
   - Live projection updates (if price reaches a level)
   - Current setup status (whether a setup is forming)
4. However, it does NOT:
   - Create permanent setups (waits for bar to close)
   - Store final projection levels (waits for confirmation)
   - Move historical labels (those are fixed)

**For All Historical Bars:**
- All bars in the past are confirmed
- They are processed normally using their final, confirmed data
- Their setups, projections, and labels never change

**3. How Setup Information is Stored (Permanent Storage):**

**Initialization (When Indicator First Loads):**
1. When the indicator is first added to your chart, it creates storage areas in memory
2. These storage areas will hold information about all your setups
3. The system creates separate storage for:
   - Setup prices (where each setup is located)
   - Bar indices (which bar each setup was confirmed on)
   - Setup states (gray, red, or orange)
   - Setup directions (bullish or bearish)
   - C2 prices (first structural point)
   - C3 prices (second structural point, if it exists)
   - C4 prices (third structural point, if it exists)
   - CISD prices (change in state of delivery point)
4. This initialization happens only once when the indicator loads

**Storing a New Setup (When Setup is Confirmed):**
1. **Wait for Confirmation:**
   - The system waits until a bar closes (becomes confirmed)
   - It does NOT store setups on bars that are still forming

2. **Check if Setup is Valid:**
   - The system checks if a valid setup has been detected
   - If yes, proceed to store it

3. **Store All Setup Information:**
   - The system saves all the important details about this setup:
     - The reference price (where the setup is based)
     - The bar number where it was confirmed (this never changes)
     - Initial state: "gray" (valid setup)
     - Direction: "bullish" or "bearish"
     - C2 price level
     - C3 price level (if C3 was formed)
     - C4 price level (if C4 was formed)
     - CISD price level
   - All this information is saved permanently

4. **Critical Rule - Data Never Changes:**
   - Once a setup is stored for a specific bar, that setup's data NEVER changes
   - Even as new bars form in the future, this historical setup remains exactly the same
   - The bar index stays the same
   - The price levels stay the same
   - The only thing that CAN change is the state (gray → red or gray → orange)
   - But even state changes only happen when new bars confirm the change
   - This ensures the indicator is non-repainting and reliable

**4. How Labels Are Positioned and Managed:**

**Critical Rules for Labels:**
- Labels represent fixed points in time that cannot move
- They mark specific bars and specific price levels
- Once placed, their position never changes

**For Each Stored Setup, the System:**

1. **Determines Label Position:**
   - **Horizontal Position (X):** Uses the bar number where the setup was confirmed
     - This bar number is fixed and never changes
     - Example: If setup was confirmed on bar #100, label stays at bar #100 forever
   
   - **Vertical Position (Y):** Uses the price level of the structural point
     - For C2 label: Uses the C2 price level
     - For C3 label: Uses the C3 price level
     - For C4 label: Uses the C4 price level
     - This price level is fixed and never changes

2. **Determines Label Text:**
   - The text is simply "C2", "C3", or "C4" depending on which point it marks
   - This text never changes

3. **Determines Label Color:**
   - The color depends on the setup's current state:
     - **Gray:** Setup is valid and active
     - **Red:** Setup has failed
     - **Orange:** Setup is in consolidation
   - Color CAN change, but only when new bars confirm a state change
   - Color does NOT change on every bar update, only when state actually changes

4. **Creates or Updates the Label:**
   - **If label doesn't exist yet:**
     - Create a new label at the fixed position
     - Use the determined text and color
     - Save the label's ID for future updates
   
   - **If label already exists:**
     - Only update the color (if state changed)
     - Position and text never change
     - The label stays exactly where it was originally placed

**Important Guarantees:**
- Label horizontal position (which bar) NEVER changes
- Label vertical position (which price) NEVER changes  
- Label text ("C2", "C3", "C4") NEVER changes
- Only label color can change (gray → red or gray → orange)
- Color change only happens when new confirmed bars prove the state change

**5. How Projections Are Calculated and Displayed:**

**Key Principle:**
- Projections are calculated from confirmed, fixed reference points
- Once calculated, the projection level (price) never changes
- Only the visual line extends forward to show where the projection is

**For Each Valid Setup (Gray State Only):**

1. **Get Fixed Reference Data:**
   - **Reference Price:** The C2 price level (where the setup began)
     - This price is fixed and never changes
   
   - **HTF Body Size:** The size of the Higher Timeframe candle body when the setup formed
     - This is measured once when the setup is confirmed
     - It never changes
   
   - **HTF Range:** The full range (high to low) of the HTF candle when setup formed
     - This is measured once when the setup is confirmed
     - It never changes

2. **Calculate Each Projection Level:**
   - For each projection you have enabled (e.g., -1, -2, -2.5, -4, -4.5):
   
   **For Body Projections:**
   - **Bullish Setup:**
     - Projections extend downward (opposite direction)
     - Formula: Projection Level = C2 Price - (Body Size × Multiplier)
     - Example: C2 = 100, Body = 2, Multiplier = -1
     - Calculation: 100 - (2 × 1) = 98
   
   - **Bearish Setup:**
     - Projections extend upward (opposite direction)
     - Formula: Projection Level = C2 Price + (Body Size × Multiplier)
     - Example: C2 = 100, Body = 2, Multiplier = -1
     - Calculation: 100 + (2 × 1) = 102
   
   **For Wick Projections:**
   - **Bullish Setup:**
     - Use HTF Low as the reference point
     - Formula: Projection Level = HTF Low - (HTF Range × Multiplier)
   
   - **Bearish Setup:**
     - Use HTF High as the reference point
     - Formula: Projection Level = HTF High + (HTF Range × Multiplier)

3. **Critical Rule - Projection Level is Fixed:**
   - Each projection level is calculated ONCE when the setup is confirmed
   - The price level never changes, even as new bars form
   - This ensures projections are reliable and non-repainting

4. **Display the Projection Line:**
   - **Create the Line:**
     - Start point: The bar where setup was confirmed (fixed)
     - Start price: The calculated projection level (fixed)
     - End point: Current bar (this extends forward as new bars form)
     - End price: Same projection level (never changes)
     - Style: Uses your settings (solid, dotted, dashed)
     - Width: Uses your setting
     - Color: Uses your setting
   
   - **Update the Line:**
     - As new bars form, only the end point extends forward
     - The start point and price level never change
     - The line grows longer but stays at the same price level

5. **Add Projection Label (if enabled):**
   - Create a text label showing the multiplier (e.g., "-1", "-2.5")
   - Place it at the setup bar and projection level
   - This helps you identify which projection is which
   - The label position never changes

**6. How to Test That the Indicator is Non-Repainting:**

**Simple Test Procedure:**
1. **Load the indicator** on your chart
2. **Note the positions** of labels and projection levels
   - Write down: Which bars have labels, what prices the projections are at
3. **Let the chart update** - allow several new bars to form
4. **Check historical elements:**
   - Historical labels should NOT have moved
   - Historical projection levels should NOT have changed
   - Only new setups should appear (if any)
5. **Check failed setups:**
   - If a setup turned red (failed), it should stay red
   - It should NOT change back to gray or orange
6. **If anything moved or changed:**
   - This indicates repainting (a problem)
   - The indicator should be fixed

**What You Should See:**
- Labels stay exactly where they were placed
- Projection lines extend forward but their price levels don't change
- New setups appear only when new bars confirm them
- Failed setups (red) remain red permanently

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

**How Timeframes Are Converted and Compared:**

**Timeframe to Minutes Conversion:**
The system converts all timeframes to minutes for easy comparison:
- 1 minute = 1 minute
- 5 minutes = 5 minutes
- 15 minutes = 15 minutes
- 30 minutes = 30 minutes
- 1 hour = 60 minutes
- 4 hours = 240 minutes
- 1 day = 1,440 minutes (24 hours × 60)
- 1 week = 10,080 minutes (7 days × 1,440)

**Calculate Timeframe Relationships:**
1. Convert Higher Timeframe to minutes (e.g., 1H = 60 minutes)
2. Convert Lower Timeframe to minutes (e.g., 5m = 5 minutes)
3. Convert current chart timeframe to minutes (e.g., 15m = 15 minutes)
4. Calculate the ratio: HTF minutes ÷ LTF minutes
   - Example: 60 ÷ 5 = 12 (1H is 12 times larger than 5m)
   - This ratio should typically be between 3 and 12 for proper fractal relationships

**How Warnings Are Displayed:**

**When Chart Timeframe is Too High:**
1. The system compares: Current chart timeframe vs. Lower Timeframe
2. If current chart timeframe is HIGHER than LTF:
   - Display warning in info table: "⚠ LTF analysis unavailable (Chart > LTF)"
   - Warning color: Orange (it's a limitation, not an error)
3. What Still Works:
   - Higher Timeframe candles still display correctly
   - HTF structure analysis continues
4. What Doesn't Work:
   - Lower Timeframe CISD detection is disabled (can't analyze LTF on higher timeframe)
   - Lower Timeframe projections are disabled
   - The system shows the warning so you know why these features aren't working

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

**How Higher Timeframe Data is Requested:**

1. **Request Data from Platform:**
   - The system asks the charting platform for data from a different timeframe than your current chart
   - It requests the following information for the Higher Timeframe candle:
     - **Open Price:** Where the HTF candle started
     - **High Price:** Highest price reached in the HTF candle
     - **Low Price:** Lowest price reached in the HTF candle
     - **Close Price:** Where the HTF candle ended
     - **Volume:** Trading volume (optional, may not always be used)
   - **Critical Setting:** Lookahead mode must be DISABLED to prevent repainting
   - This ensures only confirmed (closed) HTF candles are used

2. **How Data is Aligned:**
   - The data is matched to your current bar's time
   - Example: If you're viewing a 5-minute chart at 10:30 AM, and HTF is 1 hour:
     - The system returns the 1-hour candle that contains 10:30 AM
     - This would be the 10:00 AM - 11:00 AM hourly candle
   - If the HTF candle hasn't closed yet, it returns data from the previous closed HTF candle

3. **What the System Receives:**
   - The platform returns all the requested data in order:
     - Position 1: Open price
     - Position 2: High price
     - Position 3: Low price
     - Position 4: Close price
     - Position 5: Volume (if requested)

**How HTF Candle Metrics are Calculated:**

1. **Extract Basic Data:**
   - Open (O_HTF) = First value received
   - High (H_HTF) = Second value received
   - Low (L_HTF) = Third value received
   - Close (C_HTF) = Fourth value received

2. **Calculate Derived Measurements:**
   - **Body Size:** The difference between close and open prices
     - Formula: Absolute value of (Close - Open)
     - Example: If close is 102 and open is 100, body size = 2
   
   - **Range:** The full price movement from low to high
     - Formula: High - Low
     - Example: If high is 103 and low is 99, range = 4
   
   - **Body Midpoint:** The center of the candle body
     - Formula: (Open + Close) ÷ 2
     - Example: (100 + 102) ÷ 2 = 101
   
   - **Range Midpoint (Equilibrium):** The center of the entire candle
     - Formula: (High + Low) ÷ 2
     - Example: (103 + 99) ÷ 2 = 101
     - This is the 50% level used for premium/discount zones

3. **Calculate Candle Strength:**
   - **Body Percentage:** How much of the range is body vs. wicks
     - Formula: (Body Size ÷ Range) × 100
     - Example: If body is 2 and range is 4, body percentage = 50%
     - Higher percentage = stronger candle (more body, less wick)

4. **Determine Candle Direction:**
   - **Bullish Candle:** Close is higher than open
     - Example: Open = 100, Close = 102 → Bullish
   
   - **Bearish Candle:** Close is lower than open
     - Example: Open = 102, Close = 100 → Bearish
   
   - **Doji Candle:** Open and close are very close together
     - Body is less than 10% of the total range
     - Example: If range is 4, body must be less than 0.4 to be a doji
     - Doji candles often signal potential reversals

**How HTF Candles are Plotted on Your Chart:**

1. **Determine Which Lower Timeframe Bars Belong to Each HTF Candle:**
   - The system looks at the time period for each HTF candle
   - Example: If HTF is 1 hour, one HTF candle covers 10:00 AM to 11:00 AM
   - It then identifies all LTF bars (e.g., 5-minute bars) that fall within that time period
   - All 5-minute bars from 10:00 to 10:55 belong to that 1-hour HTF candle

2. **Plot the HTF Candle:**
   - For each LTF bar that belongs to an HTF candle:
     - Draw the HTF candle showing:
       - Open price (where it started)
       - High price (top of the wick)
       - Low price (bottom of the wick)
       - Close price (where it ended)
     - Position it at the current bar location (plus any offset you set)
     - Use the candle size you specified
     - Color it based on whether it's bullish (green/blue) or bearish (red)

**How HTF Candle Offset Works:**

1. **Apply User Offset Setting:**
   - You can shift HTF candles left or right using the offset setting
   - Offset range: -50 to +50 bars
   - Positive offset: Shifts candles to the right (forward in time)
   - Negative offset: Shifts candles to the left (backward in time)

2. **Calculate Final Position:**
   - Start with the current bar's position
   - Add (or subtract) the offset value
   - Example: Bar #100 with offset +5 = Position at bar #105

3. **Boundary Checking:**
   - The system ensures candles don't go off the chart
   - If calculated position is before the first bar: Use first bar position
   - If calculated position is after the last bar: Use last bar position
   - This keeps all HTF candles visible on your chart

**How HTF Lookback History Limits Work:**

1. **Get Your Setting:**
   - The system reads how many HTF candles you want to see (1 to 100)
   - This is your "lookback history" setting

2. **Calculate How Many to Show:**
   - Compare your setting to how many HTF candles are actually available
   - Use whichever is smaller
   - Example: You set 50, but only 30 are available → Show 30
   - Example: You set 20, and 100 are available → Show 20

3. **Display Only Recent Candles:**
   - The system shows only the most recent N HTF candles (where N is your setting)
   - It counts backward from the current HTF candle
   - Example: If you set 20, it shows the 20 most recent HTF candles
   - Older candles beyond your setting are not displayed (keeps chart clean)

### 13.2 CISD Detection Logic - Detailed Algorithm

**How Bullish CISD (Change in State of Delivery) is Detected:**

1. **Monitor Lower Timeframe Candles:**
   - The system watches all the LTF candles that are forming within the current HTF candle
   - Example: If HTF is 1 hour and LTF is 5 minutes, it watches all 5-minute candles in that hour

2. **Identify Bearish Sequence:**
   - Look for a series of bearish LTF candles (where close is below open)
   - These candles should be making lower lows, showing bearish momentum
   - Need at least 2 bearish candles to establish a significant low
   - Track the lowest price reached during this bearish sequence
   - This becomes the "significant low" point

3. **Wait for CISD Confirmation:**
   - After the bearish sequence, watch for the first LTF candle that closes ABOVE its open
   - This is the key signal: A close above open after a bearish sequence
   - This candle is the "CISD candle" - it marks the change from bearish to bullish delivery

4. **Mark the CISD:**
   - The CISD price level is typically the significant low (or the low of the CISD candle itself)
   - Draw a marker on the chart at:
     - The bar where the CISD candle occurred
     - The CISD price level
   - Use the bullish CISD color (usually green or blue)
   - Use the line style and width you specified

**How Bearish CISD is Detected:**

1. **Monitor Lower Timeframe Candles:**
   - Same as above - watch all LTF candles within the current HTF candle

2. **Identify Bullish Sequence:**
   - Look for a series of bullish LTF candles (where close is above open)
   - These candles should be making higher highs, showing bullish momentum
   - Need at least 2 bullish candles to establish a significant high
   - Track the highest price reached during this bullish sequence
   - This becomes the "significant high" point

3. **Wait for CISD Confirmation:**
   - After the bullish sequence, watch for the first LTF candle that closes BELOW its open
   - This is the key signal: A close below open after a bullish sequence
   - This candle is the "CISD candle" - it marks the change from bullish to bearish delivery

4. **Mark the CISD:**
   - The CISD price level is typically the significant high (or the high of the CISD candle itself)
   - Draw a marker on the chart at:
     - The bar where the CISD candle occurred
     - The CISD price level
   - Use the bearish CISD color (usually red)
   - Use the line style and width you specified

**How Early CISD Detection Works:**

1. **Less Strict Criteria:**
   - Early CISD uses similar logic but with less strict requirements
   - It looks for the first sign of potential orderflow change

2. **For Bullish Early CISD:**
   - After a bearish sequence, if the first LTF candle closes above its open
   - This is flagged as "Early CISD" (not yet confirmed)
   - It's marked with a distinct color (different from confirmed CISD)
   - It's shown with a different line style (often dotted) to indicate it's early

3. **Confirmation:**
   - Early CISD becomes confirmed CISD when the full criteria are met
   - Once confirmed, it uses the regular CISD color and style
   - This gives you an earlier warning of potential setup formation

### 13.3 Equilibrium Calculation - Detailed

**How Current HTF Equilibrium is Calculated:**

1. **Simple Midpoint Formula:**
   - Take the HTF candle's high price and low price
   - Add them together
   - Divide by 2
   - Formula: (High + Low) ÷ 2
   - Example: If high is 103 and low is 99, equilibrium = (103 + 99) ÷ 2 = 101

2. **Display the Equilibrium Line:**
   - Draw a horizontal line at the equilibrium price level
   - Extend it across the entire HTF candle period
   - Use the line style, width, and color you specified in settings
   - This line marks the 50% midpoint of the HTF range

**How Previous HTF Equilibrium is Calculated:**

1. **Get Previous HTF Candle Data:**
   - Request data from the previous (already closed) HTF candle
   - Get its high price and low price
   - Use only confirmed data (non-repainting)

2. **Calculate Previous Equilibrium:**
   - Use the same formula: (Previous High + Previous Low) ÷ 2
   - This gives you the equilibrium of the previous HTF candle

3. **Display Previous Equilibrium (if enabled):**
   - If you have "Show Previous Candle EQ" enabled:
     - Draw a horizontal line at the previous equilibrium level
     - Use a different style/color to distinguish it from current equilibrium
     - This provides reference for premium/discount zones from the previous structure

**How Premium and Discount Zones Work:**

1. **Premium Zone (Upper 50%):**
   - **Top:** HTF candle high price
   - **Bottom:** Current HTF equilibrium (midpoint)
   - This is the "expensive" part of the range
   - **Trading Rule:** Best short setups form in the premium zone
   - Concept: "Sell when it's expensive"

2. **Discount Zone (Lower 50%):**
   - **Top:** Current HTF equilibrium (midpoint)
   - **Bottom:** HTF candle low price
   - This is the "cheap" part of the range
   - **Trading Rule:** Best long setups form in the discount zone
   - Concept: "Buy when it's cheap"

3. **Why This Matters:**
   - The equilibrium line divides the HTF range into two equal halves
   - Price action above equilibrium = premium zone (favor shorts)
   - Price action below equilibrium = discount zone (favor longs)
   - This helps you align your trades with the HTF structure

### 13.4 Projection Calculation - Detailed Formulas

**How Body Projections are Calculated for Bullish Setups:**

1. **Identify Reference Point:**
   - Use the C2 price level as the starting point
   - This is the first structural point in the bullish setup

2. **Get HTF Body Size:**
   - Calculate the size of the HTF candle body
   - Formula: Absolute value of (Close - Open)
   - Example: If close is 102 and open is 100, body size = 2

3. **Calculate Each Projection Level:**
   - For each projection you have enabled (e.g., -1, -2, -2.5, -4, -4.5):
     - Take the multiplier value (ignore the negative sign for calculation)
     - Formula: Projection Level = C2 Price - (Body Size × Multiplier)
     - **Why subtract?** Because in bullish setups, projections extend downward (opposite direction)
   
   **Example Calculations:**
   - C2 = 100, Body = 2, Multiplier = -1
     - Projection = 100 - (2 × 1) = 98
   - C2 = 100, Body = 2, Multiplier = -2
     - Projection = 100 - (2 × 2) = 96
   - C2 = 100, Body = 2, Multiplier = -2.5
     - Projection = 100 - (2 × 2.5) = 95

4. **Display the Projection:**
   - Draw a horizontal line at each calculated projection level
   - Label it with the multiplier value (e.g., "-1", "-2")
   - Use your specified line style, width, and color

**How Body Projections are Calculated for Bearish Setups:**

1. **Identify Reference Point:**
   - Use the C2 price level as the starting point
   - This is the first structural point in the bearish setup

2. **Get HTF Body Size:**
   - Same calculation: Absolute value of (Close - Open)

3. **Calculate Each Projection Level:**
   - For each enabled projection:
     - Formula: Projection Level = C2 Price + (Body Size × Multiplier)
     - **Why add?** Because in bearish setups, projections extend upward (opposite direction)
   
   **Example Calculations:**
   - C2 = 100, Body = 2, Multiplier = -1
     - Projection = 100 + (2 × 1) = 102
   - C2 = 100, Body = 2, Multiplier = -2
     - Projection = 100 + (2 × 2) = 104

4. **Display the Projection:**
   - Same as bullish: Draw line, add label, use your settings

**How Wick Projections are Calculated for Bullish Setups:**

1. **Identify Reference Point:**
   - Use the HTF Low (the bottom wick extreme) as the starting point
   - This is the lowest price reached in the HTF candle

2. **Get HTF Range:**
   - Calculate the full range: High - Low
   - Example: If high is 103 and low is 99, range = 4

3. **Calculate Each Projection Level:**
   - For each enabled projection:
     - Formula: Projection Level = HTF Low - (HTF Range × Multiplier)
     - Projections extend downward from the HTF low
   
   **Example Calculations:**
   - HTF Low = 98, Range = 4, Multiplier = -1
     - Projection = 98 - (4 × 1) = 94
   - HTF Low = 98, Range = 4, Multiplier = -2
     - Projection = 98 - (4 × 2) = 90

4. **Display the Projection:**
   - Draw line and label with "W" prefix (e.g., "W-1", "W-2") to indicate it's a wick projection

**How Wick Projections are Calculated for Bearish Setups:**

1. **Identify Reference Point:**
   - Use the HTF High (the top wick extreme) as the starting point

2. **Get HTF Range:**
   - Same calculation: High - Low

3. **Calculate Each Projection Level:**
   - For each enabled projection:
     - Formula: Projection Level = HTF High + (HTF Range × Multiplier)
     - Projections extend upward from the HTF high
   
   **Example Calculations:**
   - HTF High = 102, Range = 4, Multiplier = -1
     - Projection = 102 + (4 × 1) = 106

4. **Display the Projection:**
   - Same as above with "W" prefix

**How the System Chooses Reference Points for Projections:**

**Priority Order:**
1. **C2 Price** - Primary reference (always used if available)
   - This is the first structural point, so it's the most reliable reference

2. **C3 Price** - Secondary reference (if C3 exists and setup has advanced)
   - Used as alternative if setup has progressed

3. **C4 Price** - Tertiary reference (if C4 exists and setup has advanced)
   - Used if setup has progressed further

4. **CISD Price** - Fallback reference (if C2 not yet formed)
   - Used only if C2 hasn't been identified yet
   - Once C2 forms, it becomes the primary reference

**If No Valid Reference:**
- If neither C2 nor CISD exists yet, projections are skipped
- The system waits until a valid reference point is established
- This ensures projections are always based on confirmed structural points

### 13.5 T-Spot Calculation - Detailed Methodology

**How T-Spot is Calculated (General Approach):**

**What T-Spot Represents:**
- T-Spot projects where the Higher Timeframe candle wick is likely to form
- It's based on TTrades' methodology that combines multiple factors:
  1. Previous structure analysis (how previous HTF candles formed)
  2. Current momentum (recent price movement strength)
  3. Fibonacci relationships (mathematical price relationships)
  4. Orderflow patterns (how orders are flowing in the market)

**For Bullish Setups:**

1. **Analyze Previous HTF Candle:**
   - Look at the previous HTF candle's wick formation
   - Identify the previous HTF high (top of the wick)
   - Identify the previous HTF body top (higher of open or close)
   - Calculate previous wick size: Previous High - Previous Body Top
   - This shows how the previous candle's wick formed

2. **Consider Current Momentum:**
   - Analyze recent Lower Timeframe candles
   - Measure how strong the current price movement is
   - Strong momentum may extend the wick further

3. **Calculate T-Spot Level:**
   - **Method 1 (Momentum-based):**
     - Formula: Current Price + (HTF Range × Extension Factor)
     - Example: Current price = 100, HTF range = 4, Factor = 0.5
     - T-Spot = 100 + (4 × 0.5) = 102
   
   - **Method 2 (Structure-based):**
     - Formula: Previous Wick High + (Previous Wick Size × Factor)
     - Uses previous structure to project forward
   
   - The system uses whichever method is more appropriate

4. **Safety Check:**
   - Ensure T-Spot is above current price (for bullish setups)
   - If calculation puts it below current price, use fallback:
     - Fallback: Current Price + (HTF Range × 0.5)
   - This ensures T-Spot is always in the expected direction

**For Bearish Setups:**

1. **Analyze Previous HTF Candle:**
   - Look at the previous HTF candle's lower wick
   - Identify the previous HTF low (bottom of the wick)
   - Identify the previous HTF body bottom (lower of open or close)
   - Calculate previous wick size: Previous Body Bottom - Previous Low

2. **Consider Current Momentum:**
   - Same momentum analysis as bullish

3. **Calculate T-Spot Level:**
   - **Method 1:** Current Price - (HTF Range × Extension Factor)
   - **Method 2:** Previous Wick Low - (Previous Wick Size × Factor)
   
4. **Safety Check:**
   - Ensure T-Spot is below current price (for bearish setups)
   - If calculation puts it above current price, use fallback:
     - Fallback: Current Price - (HTF Range × 0.5)

**T-Spot Extension Factors:**

The extension factor determines how far the T-Spot projects. Common values:
- **Conservative (0.382):** Fibonacci level, projects shorter distance
- **Moderate (0.5):** 50% of range, balanced projection
- **Aggressive (0.618 or 1.0):** Fibonacci or full range, projects further

**Default Setting:** 0.5 (moderate, 50% of range)

**Display T-Spot:**
- Draw a marker at the calculated T-Spot level
- Use the marker style, size, and color you specified
- This marks the anticipated area where the HTF candle wick will form

**Note on T-Spot Refinement:**
- Exact T-Spot calculation may require analysis of TTrades' specific methodology
- May involve more complex algorithms considering:
  - Multiple previous HTF candles
  - Volume analysis
  - Orderflow patterns
  - Market structure relationships
- Initial implementation can use simplified approach, then refine based on testing

### 13.6 Formation Liquidity Calculation

**How Previous HTF Candles are Identified for Liquidity Marking:**

1. **Configuration:**
   - The system typically looks at the previous 2-5 HTF candles
   - More candles = more liquidity zones marked (but more chart clutter)
   - Fewer candles = cleaner chart (but less context)
   - Default: 3 previous candles

2. **Request Previous HTF Data:**
   - The system requests data for the previous N HTF candles in one batch (for efficiency)
   - For each previous candle, it gets:
     - High price
     - Low price
   - Uses only confirmed data (non-repainting)

3. **Process Each Previous Candle:**
   - For each previous HTF candle:
     - Store its high price
     - Store its low price
     - Calculate its range (high - low)
     - Calculate its midpoint (high + low) ÷ 2
     - Record when it occurred
   - Organize them from most recent to oldest

4. **Validate Data:**
   - Check that data is available for all requested candles
   - If some candles are missing data:
     - Show a warning: "Insufficient historical HTF data for liquidity marking"
     - Reduce the number of candles to mark (use only those with valid data)
   - This ensures only reliable liquidity zones are marked

**How Liquidity Zones are Marked:**

**For Bullish Setups:**
1. **Identify Previous HTF Highs:**
   - Look at each previous HTF candle's high price
   - These represent resistance levels above current price

2. **Mark as Liquidity Zones:**
   - Draw horizontal lines at each previous HTF high
   - These mark "engineered liquidity pools" where price may seek stops
   - Use your specified line style, width, and color
   - These zones represent areas where sell stops may be located

**For Bearish Setups:**
1. **Identify Previous HTF Lows:**
   - Look at each previous HTF candle's low price
   - These represent support levels below current price

2. **Mark as Liquidity Zones:**
   - Draw horizontal lines at each previous HTF low
   - These mark "engineered liquidity pools" where price may seek stops
   - Use your specified line style, width, and color
   - These zones represent areas where buy stops may be located

**Why This Matters:**
- Price often seeks liquidity at previous swing points
- These marked zones show where price might "sweep" stops before continuing
- Helps identify potential reversal or continuation areas

### 13.7 How Candle 1 Liquidity Sweep is Calculated

**How Candle 1 is Identified:**

1. **What is Candle 1:**
   - Candle 1 is the initial swing point in the formation
   - It's the first significant structural point where the setup begins

2. **For Bullish Setups:**
   - Candle 1 is the first significant low
   - Compare: CISD low vs. C2 low
   - Use whichever is lower (the first/lower one)
   - This marks the initial support level
   - Type: "Support" (price support level)

3. **For Bearish Setups:**
   - Candle 1 is the first significant high
   - Compare: CISD high vs. C2 high
   - Use whichever is higher (the first/higher one)
   - This marks the initial resistance level
   - Type: "Resistance" (price resistance level)

**How the Liquidity Sweep Marker is Displayed:**

1. **Draw the Marker:**
   - Draw a horizontal line (ray) at the Candle 1 price level
   - This line extends to the right (forward in time)
   - It marks the initial liquidity level

2. **Styling:**
   - Use the line style you specified (solid, dotted, dashed)
   - Use the line width you specified
   - Use the color you specified (typically yellow or orange)

3. **Purpose:**
   - Marks the first important liquidity level in the formation
   - Shows where price initially established the setup structure
   - Helps identify the starting point for the fractal formation

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
