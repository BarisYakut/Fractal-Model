# Fractal Model Pro (TTrades) - Requirements Document

## 1. Inputs & Parameters

This section details all user-configurable inputs/parameters for the indicator. Each input is explained with its purpose, valid range, default value, and impact on indicator behavior.

### 1.1 General Settings

#### 1.1.1 Alerts Toggle
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Master switch to enable/disable all alert functionality
- **Behavior:** When OFF, no alerts will trigger regardless of individual alert settings. When ON, individual alert conditions can be enabled/disabled separately.
- **Impact:** Does not affect indicator calculations, only alert triggering.

#### 1.1.2 History Setups
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

#### 1.1.3 Fractal Pairing Mode
- **Type:** Enum (dropdown)
- **Options:** 
  - "Automatic": System selects HTF automatically
  - "Custom": User defines HTF manually
  - "1s-1m":1 second - 1 minute timeframe pair
  - "15s-5m": 15 seconds - 5 minutes timeframe pair
  - "1m-15m": 1 minute - 15 minutes timeframe pair
  - "3m-30m": 3minutes - 30 minutes timeframe pair
  - "5m-1H": 5 minutes - 1 hour timeframe pair
  - "15m-4H": 15 minutes - 4 hours timeframe pair
  - "1H-1D": 1 hour - 1 day timeframe pair
  - "4H-1W": 4 hours - 1 week timeframe pair
  - "1D-1M": 1 day - 1 month timeframe pair
- **Default:** "Automatic"
- **Purpose:** Determines how the Higher Timeframe is selected
- **Behavior:**
  - **Automatic:** Uses predefined rules based on current chart timeframe
  - **Custom:** User must specify HTF timeframe in separate input
- **Impact:** Affects all HTF-dependent calculations and visualizations

#### 1.1.4 Custom Higher Timeframe
- **Type:** Timeframe (dropdown)
- **Options:** All TradingView supported timeframes
- **Default:** "1H" (when Custom mode selected)
- **Purpose:** User-defined HTF when Fractal Pairing Mode is set to "Custom"
- **Behavior:** Only active when Fractal Pairing Mode = "Custom"
- **Validation:** Must be greater than current chart timeframe, otherwise warning displayed
- **Impact:** Determines structural context for all fractal model calculations

#### 1.1.5 Bias Selection
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

### 1.2 HTF Candles Settings

#### 1.2.1 HTF Candles
- **Type:** Integer
- **Range:** 1 to 40
- **Default:** 4 (or as specified)
- **Purpose:** Controls how many HTF candles are plotted on chart
- **Behavior:** 
  - Lower values: Focus on recent structure
  - Higher values: More historical context and extended visibility of swing development
  - When changed, the indicator plots additional HTF candles and marks vertical lines on the chart
- **Impact:** 
  - Affects chart clarity vs. historical depth
  - Provides equilibrium tracking and cleaner framework recognition
  - Higher values may impact performance

#### 1.2.2 HTF Candle Size
- **Type:** Enum (dropdown)
- **Options:**
  - "Small"
  - "Medium"
  - "Large"
- **Default:** Small
- **Purpose:** Controls the size of HTF candles
- **Impact:** Visual only, affects readability

#### 1.2.3 HTF Candle Visibility
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Show/hide higher timeframe candles on chart
- **Behavior:** When OFF, HTF candles are not plotted but calculations still occur
- **Impact:** Affects visual display only, not calculations

#### 1.2.4 HTF Candle Offset
- **Type:** Integer
- **Range:** -50 to +50 (bars)
- **Default:** 0
- **Purpose:** Shifts HTF candles left (negative) or right (positive) relative to price bars
- **Behavior:**
  - Positive values: Shift candles to the right (future direction)
  - Negative values: Shift candles to the left (past direction)

#### 1.2.5 HTF Candle Body Color
- **Type:** Color picker
- **Default:** Green/Red
- **Purpose:** Color for HTF candle body

#### 1.2.6 HTF Candle Border Color
- **Type:** Color picker
- **Default:** Same as body or contrasting color
- **Purpose:** Border/outline color for HTF candles

#### 1.2.7 HTF Candle Wick Color
- **Type:** Color picker
- **Default:** Same as body or gray
- **Purpose:** Color for HTF candle wicks (high/low lines)

#### 1.2.8 Show HTF Open Lines
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Display horizontal line at the last HTF candle open

#### 1.2.9 HTF Open Line Style
- **Type:** Enum (dropdown)
- **Options:** "Dashed", "Dotted", "Solid"
- **Default:** "Solid"

#### 1.2.10 HTF Open Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of high/low lines

#### 1.2.11 Show HTF Vertical Lines
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Display vertical lines at each new HTF timeframe start

#### 1.2.12 HTF Vertical Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"

#### 1.2.13 HTF Vertical Line Width
- **Type:** Integer
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of vertical lines

#### 1.2.14 Show HTF High/Low Lines
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Display horizontal lines at HTF candle high and low

#### 1.2.15 HTF High/Low Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"

#### 1.2.16 HTF High/Low Line Width
- **Type:** Integer
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of high/low lines

#### 1.2.17 Show Previous Candle EQ
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display equilibrium (50% level) of previous HTF candle
- **Behavior:** Shows horizontal line at previous HTF candle midpoint

#### 1.2.18 Previous Candle EQ Color
- **Type:** Color picker
- **Default:** Gray or light blue
- **Purpose:** Color for previous candle equilibrium line

#### 1.2.19 Previous Candle EQ Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for previous candle EQ

### 1.3 Model Style Settings

#### 1.3.1 Show TTFM Labels
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display C2, C3, C4 labels on chart
- **Behavior:** When OFF, labels are hidden but calculations continue
- **Impact:** Visual only, affects chart clarity

#### 1.3.2 Label Size
- **Type:** Enum (dropdown)
- **Options:** "Tiny", "Small", "Normal", "Large", "Huge"
- **Default:** "Normal"
- **Purpose:** Size of C2/C3/C4 labels
- **Impact:** Visibility and chart clutter

#### 1.3.3 Show Candle 1 Sweep
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display liquidity sweep markers at Candle 1 (initial swing point)
- **Behavior:** Horizontal rays marking liquidity levels
- **Impact:** Visual identification of liquidity zones

#### 1.3.4 Candle 1 Sweep Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"
- **Purpose:** Line style for Candle 1 sweep markers

#### 1.3.5 Candle 1 Sweep Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of Candle 1 sweep lines

#### 1.3.6 Candle 1 Sweep Color
- **Type:** Color picker
- **Default:** Yellow or orange
- **Purpose:** Color for Candle 1 sweep markers

#### 1.3.7 Show CISD (Change in State of Delivery)
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display CISD lines marking orderflow changes
- **Behavior:** Highlights candles where delivery state changes
- **Impact:** Critical for setup identification

#### 1.3.8 CISD Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Solid"
- **Purpose:** Line style for CISD markers

#### 1.3.9 CISD Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 2
- **Purpose:** Thickness of CISD lines

#### 1.3.10 CISD Color (Bullish)
- **Type:** Color picker
- **Default:** Green or blue
- **Purpose:** Color for bullish CISD (bearish to bullish change)

#### 1.3.11 CISD Color (Bearish)
- **Type:** Color picker
- **Default:** Red
- **Purpose:** Color for bearish CISD (bullish to bearish change)

#### 1.3.12 Show Early CISD (Early C2 CISD)
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Highlight early CISD within C2 candle for earlier detection
- **Behavior:** 
  - Plots the CISD before the candle has closed, making sure you can see the developing change in state of delivery
  - Shows potential CISD before full confirmation
  - Also referred to as "Early C2 CISD" in documentation
- **Impact:** Earlier setup identification, may have lower reliability, but provides earlier warning of potential setup formation

#### 1.3.13 Early CISD Color
- **Type:** Color picker
- **Default:** Light blue or cyan
- **Purpose:** Color for early CISD markers (distinct from confirmed CISD)

#### 1.3.14 Show Candle Equilibrium
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display 50% equilibrium level of HTF candles
- **Behavior:** Horizontal line at midpoint of HTF candle range

#### 1.3.15 Equilibrium Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for equilibrium lines

#### 1.3.16 Equilibrium Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of equilibrium lines

#### 1.3.17 Equilibrium Color
- **Type:** Color picker
- **Default:** Gray or white
- **Purpose:** Color for equilibrium lines

#### 1.3.18 Show T-Spot
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display T-Spot markers indicating anticipated wick formation areas
- **Behavior:** Marks projected areas where HTF candle wicks may form

#### 1.3.19 T-Spot Marker Style
- **Type:** Enum (dropdown)
- **Options:** "Circle", "Square", "Diamond", "Cross", "Arrow"
- **Default:** "Circle"
- **Purpose:** Visual style for T-Spot markers

#### 1.3.20 T-Spot Marker Size
- **Type:** Integer (slider)
- **Range:** 1 to 10
- **Default:** 5
- **Purpose:** Size of T-Spot markers

#### 1.3.21 T-Spot Color
- **Type:** Color picker
- **Default:** Purple or magenta
- **Purpose:** Color for T-Spot markers

### 1.4 Standard Deviation Projections Settings

#### 1.4.1 Show Projections
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Master switch for all projection levels
- **Behavior:** When OFF, no projections displayed regardless of individual settings
- **Impact:** Visual only

#### 1.4.2 Show Projection Labels
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display text labels on projection lines showing multiplier values
- **Impact:** Helps identify which projection is which

#### 1.4.3 Projection Label Size
- **Type:** Enum (dropdown)
- **Options:** "Tiny", "Small", "Normal", "Large"
- **Default:** "Small"
- **Purpose:** Size of projection labels

#### 1.4.4 Projection Type
- **Type:** Enum (dropdown)
- **Options:** "Body Projections", "Wick Projections", "Both"
- **Default:** "Both"
- **Purpose:** Determines which type of projections to calculate and display
- **Behavior:**
  - **Body Projections:** Based on HTF candle body size
  - **Wick Projections:** Based on HTF candle wick extremes
- **Impact:** Affects calculation method and visual display

#### 1.4.5 Projection Standard Deviation
- **Type:** Text
- **Default:** -1, -2, -2.5, -4, -4.5
- **Purpose:** Standard deviations for the projections
- **Behavior:** Multiplies reference range to calculate projection distance
- **Important:** Input must be comma separated

#### 1.4.6 Projection Color
- **Type:** Color picker
- **Default:** White or gray
- **Purpose:** Color for projection drawings

#### 1.4.7 Projection Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for all projection lines

#### 1.4.8 Projection Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of projection lines

### 1.5 Formation Liquidity Settings

#### 1.5.1 Show Formation Liquidity
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display liquidity markers from previous candles
- **Behavior:** Marks previous swing highs and lows as liquidity zones

#### 1.5.2 Formation Liquidity Line Style
- **Type:** Enum (dropdown)
- **Options:** "Solid", "Dotted", "Dashed"
- **Default:** "Dotted"
- **Purpose:** Line style for liquidity markers

#### 1.5.3 Formation Liquidity Line Width
- **Type:** Integer (slider)
- **Range:** 1 to 5
- **Default:** 1
- **Purpose:** Thickness of liquidity marker lines

#### 1.5.4 Formation Liquidity Color
- **Type:** Color picker
- **Default:** Yellow or light orange
- **Purpose:** Color for liquidity markers

### 1.6 Time Filter Settings

#### 1.6.1 Time Filter Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable all time filters

#### 1.6.2 Apply Below
- **Type:** Enum (dropdown)
- **Default:** 1 hour
- **Purpose:** Time filters will only be applied if the current chart's timeframe is below or equal to the selected timeframe

#### 1.6.3 Custom Timezone
- **Type:** Integer (UTC offset in hours)
- **Range:** -12 to +14
- **Default:** -5 (New York Eastern Time, UTC-5)
- **Purpose:** Defines timezone for time filter calculations
- **Behavior:** 
  - Positive values = UTC+X (e.g., +2 for Central European Time)
  - Negative values = UTC-X (e.g., -5 for Eastern Time)
- **Impact:** All time filter windows use this timezone for start/end times

#### 1.6.4 Time Filter 1 Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable first custom time filter window
- **Behavior:** When enabled, setups are only shown if they occur within Time Filter 1 window

#### 1.6.5 Time Filter 1 Start
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "02:00"
- **Purpose:** Start time for first time filter window

#### 1.6.6 Time Filter 1 End
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "05:00"
- **Purpose:** End time for first time filter window

#### 1.6.7 Time Filter 2 Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable second custom time filter window
- **Behavior:** When enabled, setups are only shown if they occur within Time Filter 2 window (or Time Filter 1 if both enabled)

#### 1.6.8 Time Filter 2 Start
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "08:00"
- **Purpose:** Start time for second time filter window

#### 1.6.9 Time Filter 2 End
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "11:00"
- **Purpose:** End time for second time filter window

#### 1.6.10 Time Filter 3 Enable
- **Type:** Boolean (checkbox)
- **Default:** OFF
- **Purpose:** Enable/disable third custom time filter window

#### 1.6.11 Time Filter 3 Start
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "14:00"
- **Purpose:** Start time for third time filter window

#### 1.6.12 Time Filter 3 End
- **Type:** Time (HH:MM format)
- **Range:** 00:00 to 23:59
- **Default:** "17:00"
- **Purpose:** End time for third time filter window

### 1.7 Info Table Settings

#### 1.7.1 Show Info Table
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display information table on chart
- **Behavior:** Shows current fractal model state and settings
- **Impact:** Provides quick reference information

#### 1.7.2 Info Table Position
- **Type:** Enum (dropdown)
- **Options:** "Top Left", "Top Right", "Top Center", "Bottom Left", "Bottom Right", "Bottom Center", "On Chart"
- **Default:** "Top Left"
- **Purpose:** Location of info table on chart

#### 1.7.3 Info Table Size
- **Type:** Enum (dropdown)
- **Options:** "Small", "Normal", "Large"
- **Default:** "Normal"
- **Purpose:** Size of info table text and cells

#### 1.7.4 Show Table Borders
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display borders around table cells

#### 1.7.5 Show Asset Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display current ticker info in table

#### 1.7.6 Show Chart Timeframe Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display current chart's timeframe in table

#### 1.7.7 Show Fractal Pairing Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display current HTF-LTF pairing in table

#### 1.7.8 Show Time to Close
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display countdown to next HTF candle close
- **Behavior:** Shows time remaining until current HTF candle closes

#### 1.7.9 Show Bias Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display current bias setting in table

#### 1.7.10 Show Time Filter Info
- **Type:** Boolean (checkbox)
- **Default:** ON
- **Purpose:** Display active time filter windows in table
- **Behavior:** Shows which time filters are enabled and their windows


---

## 2. Core Concept & Framework

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
- **Mechanics:** This shift in delivery confirms that a reversal is unfolding. **Wick size:** A small wick on C2 supports expansion (price can trend in one direction); a large opposing wick does not support expansion in that candle—expansion may be deferred to C3.
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

**Candle Numbering (from methodology):**
- **C2** is always the swing low or swing high (the reversal point).
- **C3** is the candle following C2.
- **C4** is the candle following C3.
- **C1** is always the candle before C2.
- Candles 1, 2, 3 form the swing; C4 is the continuation if the trend is correct.

**Two Closure Types – C2 vs C3:**

The indicator detects two distinct closure types; both can trigger a valid setup:

1. **Candle 2 (C2) closure – reversal closure:**
   - **Bullish:** Price sweeps out the previous candle's low, then closes back **above** that candle's low (close back inside the range).
   - **Bearish:** Price sweeps out the previous candle's high, then closes back **below** that candle's high (close back inside the range).
   - This is a **reversal** closure: the sweep and close-back-in confirm the reversal in C2.
   - After a C2 closure, the model anticipates a C3 continuation and then C4 if C3 closes strong.

2. **Candle 3 (C3) closure – when there is no C2 closure:**
   - Price reaches a key level but **does not** produce a reversal closure in C2 (does not close back inside the previous candle's range).
   - **Bullish C3 closure:** C3 does **not** take out C2's low, but **closes over the body** of C2 (i.e. over the opening price of C2), engulfing C2. Then C4 continuation higher is anticipated.
   - **Bearish C3 closure:** C3 does **not** take out C2's high, but **closes below the body** of C2 (i.e. below the opening price of C2), engulfing C2. Then C4 continuation lower is anticipated.
   - So: C3 closure = no C2 reversal, but C3 closes over/under C2's **body (opening price of C2)**; then look for C4 continuation.

**Setup validation (both required):**
- A **Higher Timeframe** candle 2 or candle 3 closure (as above), **and**
- A **Change in State of Delivery (CISD)** on the **Lower Timeframe** inside that HTF candle (close through the series of candles that made the high or low).
- If there is an HTF closure but no LTF CISD, the setup is not valid. If there is LTF CISD but no HTF C2/C3 closure, the setup is not valid.

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
- **Custom pairings:** User-defined HTF-LTF combinations
  - Any valid timeframe combination where HTF > LTF
  - Examples: 1H-3m, 1D-15m (allows flexibility for personal framework preferences)
  - Must respect TradingView timeframe hierarchy
- **Automatic pairing:** System automatically selects appropriate HTF based on current chart timeframe
  - Uses predefined rules to select optimal HTF
  - Ensures proper fractal relationship
  - Can be overridden with custom pairing if desired

**Important Limitation:**
- If viewing a chart timeframe **greater than the LTF** (e.g. 5m–15m model but chart is on 5m or above LTF; or 1m–15m model on 5m chart), the indicator should display a warning/error (e.g. in the info table): chart must be at or below the model’s LTF
- HTF candle plotting remains visible and functional when applicable
- LTF CISD detection will not render when chart TF > LTF (cannot detect CISD on higher timeframe)
- LTF projections will not render when chart TF > LTF
- This is a fundamental limitation: you cannot analyze LTF structure when viewing HTF or higher
- **Early failure signal:** A setup turns **red** (fails) when price **closes over** the level (e.g. in the first candle), indicating continuation is unlikely

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
- **Equilibrium is measured from wick high to wick low** (full candle range), not body; the 0.5 level is the midpoint of that range
- Can be viewed on both HTF candle and chart directly

### 3.2 Change in State of Delivery (CISD)

**Purpose:** Identify the first potential change in orderflow, signaling a shift from sell program to buy program or vice versa.

**Mechanics:**
- Marks the series of candles making up significant highs or lows
- **CISD Confirmation:** A close beyond the opening price signals a change from bullish to bearish (or vice versa)
  - **Bearish CISD:** Up-close candle(s) get closed below → use the **opening price of the first** up-close candle in the series as the CISD level
  - **Bullish CISD:** Down-close candle(s) get closed above → use the **opening price of the first** down-close candle in the series as the CISD level
  - When there is a **series** of such candles (multiple up-close or down-close in a row), the CISD level is always the **opening price of the first candle** in that series
- This confirms a trend reversal and is a form of orderblock
- **Early C2 CISD:** Option to highlight early CISD within the C2 candle for earlier detection
  - Plots the CISD before the candle has closed, allowing you to see the developing change in state of delivery
  - Provides earlier warning of potential setup formation
  - Becomes confirmed CISD when the candle closes and full criteria are met

**Visual Requirements:**
- Customizable line style and width
- Clear marking of CISD candles on the chart

### 3.3 TTFM Labels (C2/C3/C4)

**Purpose:** Identify key structural points in the fractal formation.

**Label States & Logic:**

1. **Gray Label (Valid Setup):**
   - Setup remains valid through C4
   - All projections, Equilibrium, Liquidity Sweep, and T-spot are plotted
   - Indicates stable conditions and active setup

2. **Red Label (Failed Setup – first candle):**
   - Setup fails **within the first** higher timeframe candle after the setup forms (e.g. price closes over the level in that first candle)
   - Early warning that continuation is unlikely
   - Indicator stops plotting: projections, Equilibrium, Liquidity Sweep, and T-spot
   - Labels (C2, C3, C4) remain on chart but turn red
   - Clearly indicates setup failure

3. **Orange Label (Failed Setup – second candle after):**
   - Setup does **not** fail in the first candle, but fails in the **second** candle after the setup formation
   - Label turns orange (distinct from red “first-candle” failure)
   - Signals failure occurred one candle later than red
   - Suggests market may enter consolidation or range after the second-candle failure

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
- Highlights **previous candle’s or previous day’s highs/lows that have not yet been taken** (e.g. dotted lines for “not yet taken”)
- For active setups, the formation liquidity shows liquidity from the swing point at C1
- Example: In an active bearish model, the formation liquidity is from the swing point at C1; in bullish, previous lows not yet taken
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
     - Open
     - High
     - Low
     - Close
     - Body size
     - Range
     - Body midpoint
     - Range midpoint (Equilibrium)

2. **HTF Candle Classification:**
   - **Bullish HTF Candle:** Close >= Open
   - **Bearish HTF Candle:** Close < Open

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

13. **Second-Candle Failure (Orange) Detection Logic:**

    **Orange Label (fail in second candle after setup):**
    - Setup did **not** fail in the **first** HTF candle after formation (so it does not turn red).
    - Setup **does** fail in the **second** HTF candle after formation (e.g. price closes beyond the level in that second candle).
    - **Actions:** Change all labels (C2, C3, C4) to **Orange** color.
    - Orange distinguishes “failed in second candle” from “failed in first candle” (red).
    - Stop plotting projections and markers as for failure (orange = still a failure, just one candle later).

    **Timing:** Red = failure in first candle; Orange = failure in second candle after the setup formation.

#### Phase 5: Projection & Level Calculation

14. **Projection Calculation (Only for Valid Setups - Gray Labels):**

    **Reference Points:**
    - Primary: C2 price level
    - Secondary: C3 price level
    - Tertiary: C4 price level (if exists)
    - CISD price level (for some projection types)

    **Body Projections Calculation:**
    - **Reference Range:** HTF candle body size = `abs(C_HTF - O_HTF)` (bodies in CISD used as reference where applicable)
    - **For Bullish Setup:**
      - Projection_Level = C2_Price - (Body_Size × Multiplier)
      - Negative multipliers extend downward (opposite direction)
      - Example: C2 = 100, Body = 2, Multiplier = -1 → Projection = 98
    - **For Bearish Setup:**
      - Projection_Level = C2_Price + (Body_Size × Multiplier)
      - Negative multipliers extend upward (opposite direction)
      - Example: C2 = 100, Body = 2, Multiplier = -1 → Projection = 102
    - **Manipulation-leg projections (methodology):** At C2 reversal point, projections can be derived from the manipulation leg: find the low (bearish) or high (bullish), then the high that made that low or the low that made that high; anchor the Fib from that leg. Standard levels: 1, 0, -1, -2, -2.5, -4, -4.5 (user may add levels, e.g. manual input “,5” for -5). Normal manipulation leg: -2 to -2.5; expansion: -4 to -4.5; large manipulation leg: use -1.

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
    - **Methodology:** Based on TTrades' refined analysis. The T-Spot is the area where the HTF candle’s wick is expected to form (e.g. short setup: price opens high, goes lower, closes → anticipate the high in that box). It can align with the 0.5/EQ range of the relevant HTF candle.
    - **General Approach:**
      - Analyze previous HTF candle wick formation
      - Consider current momentum and structure
      - Project where next HTF candle wick is likely to form
    - **For Bullish Setup:** T-Spot typically projects above current price; anticipates upper wick formation area.
    - **For Bearish Setup:** T-Spot typically projects below current price; anticipates lower wick formation area.
    - Draw marker at T-Spot level; use specified marker style, size, and color.

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

1. **Wait for first HTF candle after setup formation to close:**
   - If price closes beyond the level (invalidating the setup) **in that first candle** → mark as **Red** (first-candle failure). Stop projections and markers.

2. **If the first candle did NOT fail the setup, then evaluate the second candle:**
   - After the **second** HTF candle after setup formation closes, check whether price has closed beyond the level in that second candle.
   - If yes → **Orange** (second-candle failure). Stop projections and markers. Setup is invalidated.
   - If no → setup remains **Gray** (still valid).

**Consolidation vs. Failure:**
- **Second-candle failure (Orange):** Setup failed in the second candle after formation; distinct from first-candle failure (red).
- **Failure (Red):** Setup invalidated. All projections stop. Setup is dead.

**Failure (Red and Orange):**
- Red = failed in first candle; Orange = failed in second candle. Both invalidate the setup; projections and markers stop. Setup does not recover to gray once failed.

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

Each time filter consists of four pieces of information:

1. **Enabled Status:** Whether this filter is currently active (on or off)
2. **Start Time:** The beginning of the time window (in HH:MM format, e.g., "02:00" or "08:00")
3. **End Time:** The end of the time window (in HH:MM format, e.g., "05:00" or "11:00")
4. **Timezone Offset:** The UTC offset in hours (e.g., -5 for Eastern Time, +2 for Central European Time)

**Up to Three Time Filters:**
- You can define up to three separate time filters
- Each filter operates independently (documentation also refers to these as “kill zones,” up to 3)
- Example configurations:
  - **Time Filter 1:** Enabled = OFF, Start = 02:00, End = 05:00, Timezone = -5
  - **Time Filter 2:** Enabled = OFF, Start = 08:00, End = 11:00, Timezone = -5
  - **Time Filter 3:** Enabled = OFF, Start = 14:00, End = 17:00, Timezone = -5
- **Whole-hour candle logic (sessions):** When a session is defined by a range (e.g. 8:00–11:30 NY), to include the candle that **opens** at 8:00, set the filter start to **7:00** (or earlier) so that the 8:00 open falls inside the window.

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

**How Multiple Time Filters Work Together:**

When multiple time filters are enabled, the system uses "OR" logic:
- A setup is shown if it falls within **ANY** of the enabled filters
- You don't need to be in ALL filters - just ONE is enough

**Example with Multiple Filters:**
- Filter 1: 02:00-05:00 (enabled)
- Filter 2: 08:00-11:00 (enabled)
- Filter 3: 14:00-17:00 (disabled)

**Results:**
- Setup at 03:00 → **Shown** (within Filter 1) ✓
- Setup at 09:30 → **Shown** (within Filter 2) ✓
- Setup at 15:00 → **NOT shown** (Filter 3 is disabled, and 15:00 is not in Filter 1 or 2)
- Setup at 06:00 → **NOT shown** (not in any enabled filter)

**How Timezone Conversion Works:**

1. **Get Your Timezone Setting:**
   - You specify your timezone offset (e.g., -5 for Eastern Time, +2 for Central European Time)
   - This tells the system how many hours to adjust from UTC

2. **Convert UTC to Your Local Time:**
   - The system gets the current time in UTC (Coordinated Universal Time)
   - It adds your timezone offset to convert to your local time
   - Formula: Local Time = UTC Time + Timezone Offset
   
   **Example 1 - New York Eastern Time (UTC-5):**
   - UTC time: 10:00
   - Your timezone: -5 hours
   - Calculation: 10:00 + (-5) = 05:00
   - Your local time: 5:00 AM
   
   **Example 2 - London Time:**
   - UTC time: 10:00
   - In winter: Timezone = +0 hours (GMT)
     - Local time: 10:00 (same as UTC)
   - In summer: Timezone = +1 hour (BST - British Summer Time)
     - Local time: 11:00 (one hour ahead)

3. **Daylight Saving Time (DST) Handling:**
   - Some timezones change by 1 hour during DST periods
   - The system may automatically detect DST based on the current date
   - If automatic detection is enabled:
     - During DST period: Add 1 hour to base timezone offset
     - Outside DST period: Use base timezone offset
   - Alternatively, you may need to manually adjust your timezone setting when DST changes occur
   - Example: Eastern Time is UTC-5 in winter, but UTC-4 in summer (during DST)

**Common Trading Session Filter Examples:**

**London Session (European Markets Open):**
- Start: 08:00 (8:00 AM London time)
- End: 12:00 (12:00 PM London time)
- Timezone: UTC+0 (GMT) or UTC+1 (BST in summer)
- Purpose: Focus on European market hours

**New York Session (US Markets Open):**
- Start: 13:30 (1:30 PM London time, which is 8:30 AM EST)
- End: 20:00 (8:00 PM London time, which is 3:00 PM EST)
- Timezone: UTC-5 (EST) or UTC-4 (EDT during DST)
- Purpose: Focus on US market hours

**Asian Session (Tokyo/Hong Kong):**
- Start: 00:00 (Midnight London time)
- End: 09:00 (9:00 AM London time)
- Timezone: UTC+9 (Tokyo time)
- Purpose: Focus on Asian market hours

**Forex Overlap Periods (Highest Volatility):**
- London-New York Overlap:
  - Start: 13:30 (When both sessions are open)
  - End: 16:00
  - Timezone: UTC+0 or UTC-5 (depending on which session you're tracking)
  - Purpose: Focus on highest volatility periods when multiple markets are active

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
- Setup second-candle failure (orange)
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
     - **Orange:** Setup failed in second candle after formation
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

**How HTF-LTF Relationships Are Managed:**

**Step 1: Get Current Timeframe Information:**
1. The system reads the current chart timeframe
   - Example values: "5" (5 minutes), "15" (15 minutes), "60" (1 hour), "240" (4 hours)
   - This tells the system what timeframe you're currently viewing

2. The system reads the Higher Timeframe setting
   - Example: "60" for 1 hour
   - This is what you selected (or what was auto-selected)

3. The system determines the Lower Timeframe from the pairing
   - Example: If HTF is 1H (60 minutes) and pairing is 5m-1H, then LTF is 5 minutes
   - The LTF is determined by the fractal pairing relationship

**Step 2: Validate the Timeframe Relationship:**

**Check 1: Is Current Timeframe Compatible with LTF?**
1. Compare: Current chart timeframe vs. Lower Timeframe
2. If current timeframe is HIGHER than LTF:
   - **Problem:** Cannot analyze LTF structure when viewing a higher timeframe
   - **Action:** Show warning message: "LTF analysis unavailable. Chart timeframe > LTF."
   - **Result:** LTF analysis is disabled (cannot analyze LTF on higher timeframe)
   - **Note:** This is a warning, not an error - HTF candles still work
3. If current timeframe is EQUAL TO or LOWER than LTF:
   - **Result:** LTF analysis is available
   - The system can properly analyze LTF structure

**Check 2: Is HTF Greater Than LTF?**
1. Compare: Higher Timeframe vs. Lower Timeframe
2. If HTF is NOT greater than LTF (HTF ≤ LTF):
   - **Problem:** Invalid pairing - HTF must always be greater than LTF
   - **Action:** Show error message: "HTF must be greater than LTF"
   - **Result:** Return false - pairing is invalid
   - **Impact:** Indicator cannot function with this pairing
3. If HTF IS greater than LTF:
   - **Result:** Return true - pairing is valid
   - The indicator can proceed with calculations

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

**1. How Setup Storage and Array Management Works:**

**Initialization (When Indicator First Loads):**

1. **Check if Storage is Initialized:**
   - When the indicator first loads, it checks if setup storage has been created
   - If not initialized yet, proceed to create it

2. **Create Main Storage Array:**
   - The system creates a persistent storage array that will hold all setup information
   - This array persists across bar updates (doesn't reset when new bars form)
   - It's designed to track up to 40 historical setups simultaneously

3. **Define Setup Structure:**
   - Each setup stored in the array contains the following information:
     - **Bar Index:** Which bar number the setup was confirmed on
     - **Setup ID:** A unique identifier for this specific setup
     - **Direction:** Whether it's "bullish" or "bearish"
     - **State:** Current state - "gray" (valid), "red" (failed in first candle), or "orange" (failed in second candle)
     - **C2 Price:** The C2 price level
     - **C3 Price:** The C3 price level (may be empty if C3 hasn't formed yet)
     - **C4 Price:** The C4 price level (may be empty if C4 hasn't formed yet)
     - **CISD Price:** The CISD price level
     - **HTF Body Size:** The Higher Timeframe candle body size when setup formed
     - **HTF Range:** The Higher Timeframe candle range when setup formed
     - **HTF High:** The Higher Timeframe candle high
     - **HTF Low:** The Higher Timeframe candle low
     - **Projection Levels:** All calculated projection levels for this setup
     - **Label IDs:** References to all labels created for this setup
     - **Line IDs:** References to all lines created for this setup
     - **Created Timestamp:** When this setup was first created

4. **Mark as Initialized:**
   - Once storage is created, mark it as initialized
   - This prevents recreating it on every bar update

**Managing Array Size:**

1. **Determine Maximum Setups:**
   - Read your History Setups setting (how many setups you want to see)
   - Compare this to the maximum allowed (40 setups)
   - Use whichever is smaller
   - Example: If you set 50, use 40 (the cap). If you set 10, use 10.

2. **Check Current Setup Count:**
   - Count how many setups are currently stored in the array
   - Compare this to your maximum setting

3. **Remove Old Setups if Needed:**
   - If current count exceeds your maximum:
     - Calculate how many setups to remove
     - Remove the oldest setups first (FIFO - First In First Out)
     - For each removed setup:
       - Delete all its labels from the chart
       - Delete all its lines from the chart
       - Free up the memory used by that setup
     - This keeps the array size within your limit

**Adding New Setups:**

1. **When a New Setup is Confirmed:**
   - Create a new setup object with all its information:
     - Bar index where it was confirmed
     - Direction (bullish or bearish)
     - Initial state: "gray" (valid)
     - C2 price and other price levels
     - All HTF candle data
     - Timestamp of when it was created

2. **Add to Storage Array:**
   - Add this new setup to the end of the array
   - It becomes the most recent setup

3. **Check Array Size Again:**
   - After adding, check if array size exceeds your maximum
   - If it does, remove the oldest setup (same cleanup process as above)
   - This ensures the array never exceeds your History Setups limit

**2. How Calculation Caching Works:**

**Purpose:** Some calculations are expensive (take time and resources). Instead of recalculating them on every bar, the system caches (stores) the results and reuses them until they need to be updated.

**What Gets Cached:**

1. **HTF Equilibrium Value:**
   - The equilibrium (50% midpoint) of the Higher Timeframe candle
   - This value doesn't change until the HTF candle closes
   - Formula: (HTF High + HTF Low) ÷ 2

2. **HTF Candle Bar Index:**
   - Which bar the cached equilibrium belongs to
   - Used to verify the cache is still valid

**How Caching Works:**

1. **Initialize Cache (First Time):**
   - Create storage for cached equilibrium value (starts empty)
   - Create storage for cached bar index (starts empty)

2. **Check if HTF Candle Has Closed:**
   - Monitor whether the current HTF candle has closed
   - If it has closed: Proceed to step 3
   - If it hasn't closed yet: Proceed to step 4

3. **Recalculate and Update Cache (When HTF Candle Closes):**
   - Calculate the new HTF equilibrium: (HTF High + HTF Low) ÷ 2
   - Store this value in the cache
   - Store the current bar index in the cache
   - This cache will be used until the next HTF candle closes

4. **Use Cached Value (When HTF Candle Still Forming):**
   - The HTF candle hasn't closed yet, so the equilibrium hasn't changed
   - Use the cached equilibrium value instead of recalculating
   - This saves processing time and resources

**Benefits:**
- Faster indicator execution (doesn't recalculate unnecessarily)
- Lower resource usage
- Same accuracy (value only changes when HTF candle closes anyway)

**3. How Conditional Rendering Optimizes Performance:**

**Principle:** The system only calculates and displays what you've enabled. If a feature is turned off, the system skips those calculations entirely, saving processing time and resources.

**How It Works:**

1. **Check HTF Candle Visibility Setting:**
   - If "Show HTF Candles" is enabled:
     - Calculate HTF candle data
     - Plot HTF candles on the chart
   - If "Show HTF Candles" is disabled:
     - Skip HTF candle calculations
     - Don't plot anything
     - **Note:** Calculations may still occur in background, but rendering is skipped

2. **Check Projection Settings:**
   - If "Show Projections" is enabled AND setup state is "gray" (valid):
     - Calculate projection levels
     - Plot projection lines on the chart
   - If projections are disabled OR setup has failed (red) or consolidated (orange):
     - Skip projection calculations
     - Don't plot projection lines

3. **Check SMT Divergence Setting:**
   - If "Show SMT" is disabled:
     - Skip all SMT divergence calculations
     - Don't request data for SMT pairs
     - Don't perform trend analysis
     - Don't detect divergences
   - This saves significant processing time since SMT requires data from multiple markets

**Benefits:**
- Faster indicator execution when features are disabled
- Lower resource usage
- Better chart performance
- Only processes what you actually want to see

**4. How Multi-Timeframe Data Requests Are Optimized:**

**Principle:** Each request to get data from a different timeframe has overhead (takes time and resources). The system minimizes the number of requests by batching them together and caching results.

**Inefficient Approach (What NOT to Do):**

If the system made separate requests for each piece of data, it would need to make 5 separate API calls:
1. Request HTF Open price
2. Request HTF High price
3. Request HTF Low price
4. Request HTF Close price
5. Request HTF Volume

**Problem:** Each call has overhead, making 5 calls is slow and inefficient.

**Efficient Approach (What the System Does):**

1. **Batch All Data in One Request:**
   - Make a single request that asks for all needed data at once:
     - Symbol: Current chart symbol
     - Timeframe: Higher Timeframe setting
     - Data Fields: Open, High, Low, Close, and Volume (all together)
     - Lookahead Mode: Disabled (for non-repainting)
   - The platform returns all this data in one response

2. **Extract Data from Single Response:**
   - From the single response, extract each piece of data:
     - Open price (first item in response)
     - High price (second item in response)
     - Low price (third item in response)
     - Close price (fourth item in response)
     - Volume (fifth item in response, if needed)

**Additional Optimization: Cache HTF Data**

1. **Check if HTF Candle Has Closed:**
   - Monitor whether the HTF candle has closed since the last check
   - If it just closed: Proceed to step 2
   - If it's still forming: Proceed to step 3

2. **Request New Data (When HTF Candle Closes):**
   - The HTF candle just closed, so new data is available
   - Make a batched request for all HTF data (as described above)
   - Store this data in cache
   - Update the timestamp of when the HTF candle closed
   - This cached data will be used until the next HTF candle closes

3. **Use Cached Data (When HTF Candle Still Forming):**
   - The HTF candle hasn't closed yet, so the data hasn't changed
   - Retrieve the cached HTF data instead of making a new request
   - This saves processing time and API calls

**Optimization for Previous HTF Candle Data:**

1. **Check if Previous HTF Data is Cached:**
   - The system needs previous HTF candle data for "Previous Candle EQ" calculations
   - Check if this data has already been requested and cached
   - If not cached: Proceed to step 2
   - If already cached: Use cached data (skip step 2)

2. **Request Previous HTF Data (Once):**
   - Make a request for previous HTF candle data:
     - Symbol: Current chart symbol
     - Timeframe: Higher Timeframe setting
     - Data Fields: High price and Low price (for EQ calculation)
     - Bar Offset: 1 (previous bar)
     - Lookahead Mode: Disabled
   - Store this data in cache
   - Mark it as cached so it won't be requested again

**Benefits:**
- Fewer API calls (1 instead of 5+)
- Faster execution
- Lower resource usage
- Better chart performance
- Data is only requested when it actually changes

**Performance Benefits:**
- Reduces API calls by 80% (5 calls → 1 call)
- Faster indicator execution
- Lower server load
- Better user experience

**5. How Visual Element Management Works:**

**Principle:** Charting platforms have limits on how many visual elements (labels, lines, markers) can be displayed. The system efficiently manages these elements to stay within limits while maintaining visual clarity.

**Initialization (When Indicator First Loads):**

1. **Check if Visual Element Tracking is Initialized:**
   - When the indicator first loads, check if tracking arrays have been created
   - If not initialized yet, proceed to create them

2. **Create Tracking Arrays:**
   - Create an array to track all label IDs
   - Create an array to track all line IDs
   - Create an array to track all marker IDs
   - Set maximum limits:
     - Maximum labels allowed: 500 (platform limit, may vary)
     - Maximum lines allowed: 1000 (platform limit, may vary)
   - Mark tracking as initialized

**Strategy for Visual Elements:**

The system uses different visual elements for different purposes:
- **Lines:** More efficient for drawing price levels (projections, EQ levels)
- **Labels:** Better for text annotations (C2, C3, C4 labels)
- **Markers:** Good for point identification (T-Spots, liquidity sweeps)

**Label Management:**

1. **Count Current Labels:**
   - Count how many labels are currently tracked in the array
   - Compare this to the maximum allowed (500)

2. **Remove Old Labels if Needed:**
   - If current count is at or exceeds the maximum:
     - Calculate how many labels to remove
     - Add a buffer (remove 10 extra labels to create space)
     - Remove the oldest labels first (FIFO - First In First Out)
     - For each label to remove:
       - Get the oldest label ID from the array
       - Delete that label from the chart
       - Remove its ID from the tracking array
     - This keeps the label count below the limit

**Line Management:**

1. **Count Current Lines:**
   - Count how many lines are currently tracked in the array
   - Compare this to the maximum allowed (1000)

2. **Remove Old Lines if Needed:**
   - If current count is at or exceeds the maximum:
     - Calculate how many lines to remove
     - Add a buffer (remove 50 extra lines to create space)
     - Remove the oldest lines first (FIFO)
     - For each line to remove:
       - Get the oldest line ID from the array
       - Delete that line from the chart
       - Remove its ID from the tracking array
     - This keeps the line count below the limit

**Creating New Visual Elements for a Setup:**

When a new setup is confirmed, the system creates visual elements:

1. **Create C2 Label:**
   - Position: At the bar where setup was confirmed, at C2 price level
   - Text: "C2"
   - Color: Based on setup state (gray, red, or orange)
   - Size: Based on your label size setting
   - Add this label ID to the main tracking array
   - Add this label ID to the setup's own label tracking array

2. **Create C3 Label (if C3 exists):**
   - Only create if C3 price level has been identified
   - Position: At the bar where C3 was identified, at C3 price level
   - Text: "C3"
   - Color: Based on setup state
   - Size: Based on your label size setting
   - Add to both tracking arrays

3. **Create C4 Label (if C4 exists):**
   - Only create if C4 price level has been identified
   - Position: At the bar where C4 was identified, at C4 price level
   - Text: "C4"
   - Color: Based on setup state
   - Size: Based on your label size setting
   - Add to both tracking arrays

4. **Create Projection Lines (if enabled):**
   - Create lines for body projections and wick projections
   - Add line IDs to both tracking arrays

**Benefits:**
- Prevents exceeding platform limits
- Maintains chart performance
- Automatically cleans up old elements
- Keeps visual clarity while managing resources

**Cleaning Up Visual Elements When Setup is Removed:**

When a setup is removed (because it's too old or exceeded history limit), the system cleans up all its visual elements:

1. **Delete All Labels:**
   - For each label ID associated with this setup:
     - Delete the label from the chart
     - Remove its ID from the main label tracking array
   - This prevents orphaned labels from cluttering the chart

2. **Delete All Lines:**
   - For each line ID associated with this setup:
     - Delete the line from the chart
     - Remove its ID from the main line tracking array
   - This prevents orphaned lines from cluttering the chart

3. **Delete All Markers:**
   - For each marker ID associated with this setup:
     - Delete the marker from the chart
     - Remove its ID from the main marker tracking array
   - This prevents orphaned markers from cluttering the chart

This cleanup ensures the chart stays clean and performance remains optimal.

**6. Performance Testing Requirements:**

**Test Scenarios That Must Be Verified:**

1. **Maximum History Scenario:**
   - Test with History Setups set to maximum (40 setups)
   - Verify the indicator loads and functions correctly
   - Verify all 40 setups display properly
   - Verify performance remains acceptable

2. **All Features Enabled:**
   - Enable all features simultaneously:
     - HTF candles
     - All projections
     - SMT divergence
     - All labels
     - Info table
     - Time filters
   - Verify the indicator functions correctly with everything enabled
   - Verify performance remains acceptable

3. **Multiple Timeframe Pairings:**
   - Test different HTF-LTF pairings:
     - 5m-1H
     - 15m-4H
     - 1H-1D
     - Other valid pairings
   - Verify the indicator works correctly for each pairing
   - Verify calculations are accurate for each pairing

4. **Long Chart History:**
   - Test with charts containing 1000+ bars of history
   - Verify the indicator loads within acceptable time
   - Verify all historical setups display correctly
   - Verify performance remains acceptable

5. **Real-Time Updates:**
   - Test with live market data
   - Verify the indicator updates correctly as new bars form
   - Verify there's no lag or delay in updates
   - Verify non-repainting behavior works correctly

**Performance Targets:**

1. **Chart Load Time:**
   - Target: Less than 2 seconds to fully load the indicator
   - This includes calculating all historical setups
   - This includes rendering all visual elements

2. **Real-Time Update Speed:**
   - Target: Less than 100 milliseconds per update
   - Updates occur when new bars form or existing bars update
   - Should not cause noticeable lag or delay

3. **Memory Usage:**
   - Target: Reasonable memory usage
   - Should not cause browser or platform slowdown
   - Can be monitored using platform's built-in performance tools
   - Should scale appropriately with number of setups and history length

### 12.4 Error Handling & Edge Cases

**1. How Invalid Timeframe Pairings Are Handled:**

**Detection Process:**
1. The system compares the Higher Timeframe and Lower Timeframe settings
2. It checks: Is HTF greater than LTF?
   - If NO (HTF ≤ LTF): This is invalid
   - If YES (HTF > LTF): This is valid

**When Invalid Pairing is Detected:**
1. Display error message: "HTF must be greater than LTF"
2. Disable the indicator (stop all calculations and plotting)
3. Show the error clearly so the user knows what's wrong
4. The indicator will not function until a valid pairing is selected

**When Valid Pairing is Detected:**
- The indicator continues normally
- All features work as expected

**2. How Insufficient Data Is Handled:**

**Detection Process:**
1. The system calculates how many bars are required:
   - Based on the Higher Timeframe setting
   - Based on the History Setups setting (how many past setups to show)
   - Example: If HTF is 1H and you want 10 historical setups, you need many bars of data

2. Compare Required vs. Available:
   - Count how many bars are currently available on the chart
   - Compare: Available bars vs. Required bars
   - If available bars < required bars: Insufficient data

**When Insufficient Data is Detected:**
1. Display message: "Insufficient data. Need [X] bars."
   - The message shows exactly how many bars are needed
2. Options for handling:
   - Show partial indicator (with limited functionality)
   - Wait for more data to load
   - Reduce the history settings to match available data
3. The indicator may still function but with limited historical context

**When Sufficient Data is Available:**
- The indicator functions normally
- All historical setups can be displayed

**3. How Market Gaps Are Handled:**

**What Are Market Gaps:**
- Gaps occur when there's no trading activity between bars
- Common in: Forex (weekend gaps), Stocks (overnight gaps), Crypto (occasional gaps)
- Gaps show as missing price data (no open, high, low, close values)

**Detection Process:**
1. The system checks each bar for valid price data
2. It looks for missing values:
   - If close price is missing (NA/null)
   - OR if open price is missing (NA/null)
   - Then this bar has a gap

**Handling Options:**
1. **Skip the Bar:**
   - Ignore this bar completely
   - Don't use it for any calculations
   - Move to the next valid bar

2. **Use Previous Values:**
   - Use the previous bar's values as a fallback
   - Example: If current bar's close is missing, use previous bar's close
   - This maintains continuity in calculations

**Impact:**
- Gaps don't break the indicator
- The system continues functioning despite missing data
- Calculations remain stable

**4. How SMT Pair Unavailability Is Handled:**

**Detection Process:**
1. **Test Pair 1 Data:**
   - The system tries to request price data for SMT Pair 1
   - Uses the same timeframe as your current chart
   - Requests closing price data
   - If the request fails or returns no data:
     - Pair 1 is unavailable

2. **Test Pair 2 Data:**
   - Same process for Pair 2
   - If Pair 2 data is unavailable:
     - Pair 2 cannot be used

**When Data is Unavailable:**
1. **If Pair 1 Data is Missing:**
   - Show warning: "SMT Pair 1 data unavailable"
   - Disable the SMT divergence feature
   - Stop the SMT process
   - The rest of the indicator continues working normally

2. **If Symbol is Invalid:**
   - Show error: "SMT Pair 1 symbol invalid"
   - This means the symbol you entered doesn't exist or isn't accessible
   - Disable the SMT divergence feature
   - The rest of the indicator continues working normally

**When Both Pairs Are Available:**
- SMT divergence feature is enabled
- The system can compare the two markets
- Divergence detection works normally

### 12.5 Platform Compatibility & Requirements

**Platform Version Requirements:**
- Target platform: TradingView (or specified charting platform)
- Script language version: Latest stable version
- Use modern syntax and functions
- Avoid deprecated functions that may be removed in future versions

**Required Platform Functions:**

**1. Multi-Timeframe Data Access:**

The platform must provide a function to request data from different timeframes than the current chart. This function needs to accept:
- **Symbol:** Which market to get data for
- **Timeframe:** Which timeframe to request (e.g., 1H, 4H, 1D)
- **Data Fields:** What data to get (open, high, low, close, volume)
- **Lookahead Mode:** Whether to use future data (must be disabled for non-repainting)
- **Bar Offset:** Which historical bar to get (e.g., previous bar = offset 1)

**Why This is Essential:**
- This is the core function needed for Higher Timeframe candle analysis
- Without this, the indicator cannot access HTF data
- Must support non-repainting mode (lookahead disabled)

**2. Array/Collection Management:**

The platform must provide functions for managing collections of data (arrays). Required functions:
- **Create Array:** Create a new empty array to store data
- **Add to Array:** Add an element to the end of an array
- **Remove from Array:** Remove an element at a specific position
- **Get Array Element:** Retrieve an element at a specific position
- **Get Array Size:** Count how many elements are in the array
- **Clear Array:** Remove all elements from an array

**Why This is Required:**
- Needed for managing multiple historical setups (up to 40)
- Stores setup data, prices, bar indices, states, etc.
- Essential for non-repainting data storage

**3. Visual Element Creation:**

The platform must provide functions for creating and managing visual elements on the chart.

**Label Creation and Management:**
- **Create Label:** Create a text label at a specific position (x, y) with text, color, size, and style
- **Update Label:** Modify an existing label's properties (color, text, etc.)
- **Delete Label:** Remove a label from the chart
- **Get Label Properties:** Retrieve a label's current settings

**Line Creation and Management:**
- **Create Line:** Draw a line from point (x1, y1) to point (x2, y2) with color, width, and style
- **Update Line:** Modify an existing line's properties
- **Delete Line:** Remove a line from the chart
- **Extend Line:** Extend a line to a new end point (used for projections that extend forward)

**Marker/Shape Creation:**
- **Create Marker:** Place a marker/shape at a specific position (x, y) with shape type, color, and size
- **Update Marker:** Modify an existing marker's properties
- **Delete Marker:** Remove a marker from the chart

**4. Bar State Detection:**

The platform must provide functions to determine the state of bars on the chart.

**Required Functions:**
- **Get Bar State:** Determine if a bar is CONFIRMED (closed), FORMING (currently updating), or REAL_TIME (live)
- **Is Bar Confirmed:** Check if a specific bar has closed and is final
- **Is Bar Last:** Check if this is the last (currently forming) bar
- **Is Bar Real Time:** Check if this bar is updating in real-time
- **Get Current Bar Index:** Get the index/number of the current bar
- **Get Bar Time:** Get the timestamp of when a specific bar occurred

**Why This is Critical:**
- Essential for non-repainting behavior
- Determines when to store data permanently vs. show live updates
- Prevents using unconfirmed data in calculations

**5. Table Creation (for Info Table):**

The platform must provide functions for creating and managing tables on the chart.

**Required Functions:**
- **Create Table:** Create a new table at a specific position with specified number of columns and rows
- **Set Table Cell:** Set the text and properties of a specific cell (column, row)
- **Update Table Cell:** Modify an existing cell's properties
- **Delete Table:** Remove a table from the chart

**Why This is Needed:**
- Used for the Info Table that displays fractal pairing, bias, time to close, and filters
- Provides organized information display

**6. Price Data Access:**

The platform must provide functions to access price data from the current chart.

**Required Functions:**
- **Get Open Price:** Retrieve the opening price of a specific bar
- **Get High Price:** Retrieve the highest price of a specific bar
- **Get Low Price:** Retrieve the lowest price of a specific bar
- **Get Close Price:** Retrieve the closing price of a specific bar
- **Get Volume:** Retrieve the trading volume of a specific bar
- **Get Time:** Retrieve the timestamp of when a specific bar occurred

**Why This is Essential:**
- Core data needed for all calculations
- Must be able to access historical bars (not just current)
- Required for CISD detection, projection calculations, etc.

**Compatibility Testing Requirements:**

**1. Chart Type Compatibility Testing:**

**Test on All Available Chart Types:**
The indicator must be tested on all chart types the platform supports:
- Candlestick charts
- Line charts
- Area charts
- Baseline charts
- Heikin Ashi charts
- Renko charts
- Point & Figure charts

**Testing Requirements:**
- The indicator should work correctly on all chart types
- Visual elements may need adjustment for different chart types
- For each chart type:
  - Test that the indicator loads and functions
  - Verify visual elements render correctly
  - Verify calculations are accurate
  - Ensure HTF candles display properly
  - Check that labels and lines appear correctly

**2. Asset Type Compatibility Testing:**

**Test on Different Asset Classes:**
The indicator must be tested across various asset types:
- **Stock Indices:** SPX, NASDAQ, etc.
- **Stock Futures:** ES, NQ, YM, etc.
- **Forex Pairs:** EURUSD, GBPUSD, etc.
- **Cryptocurrencies:** BTC, ETH, etc.
- **Commodities:** Gold, Oil, etc.
- **Bonds:** 10Y Treasury, etc.

**Why Different Assets Matter:**
Each asset class may have different characteristics:
- **Trading Hours:** Some trade 24/7, others have specific sessions
- **Price Precision:** Different decimal places (forex vs. stocks)
- **Volume Characteristics:** Some have volume, others don't
- **Gap Behavior:** Some have frequent gaps, others don't

**Testing Requirements:**
For each asset class:
- Test that time filters work correctly with that asset's trading hours
- Verify price precision is handled correctly
- Verify gap handling works appropriately
- Ensure all calculations are accurate for that asset type

**3. Timeframe Compatibility Testing:**

**Test on All Common Timeframes:**
The indicator must be tested on:
- 1 minute
- 5 minutes
- 15 minutes
- 30 minutes
- 1 hour
- 4 hours
- 1 day
- 1 week

**Why Different Timeframes Matter:**
Different timeframes may have:
- **Different Data Availability:** Some timeframes have more historical data
- **Different Update Frequencies:** Lower timeframes update more often
- **Different Precision Requirements:** Higher timeframes may need different calculations

**Testing Requirements:**
For each timeframe:
- Test that HTF-LTF pairing works correctly
- Verify calculations are accurate
- Verify performance is acceptable (doesn't slow down chart)
- Test that all features function properly on that timeframe

**4. Mobile App Compatibility Testing:**

**If Platform Has Mobile App:**
The indicator must be tested on mobile devices:
- iOS iPhone
- iOS iPad
- Android Phone
- Android Tablet

**Testing Requirements:**
For each device:
- Test that the indicator loads and functions
- Verify visual elements display correctly (labels, lines, candles)
- Verify touch interactions work (if applicable)
- Verify performance is acceptable (doesn't lag or freeze)
- Verify battery usage is reasonable (doesn't drain battery excessively)

**5. Browser Compatibility Testing:**

**Test on Different Web Browsers:**
The indicator must be tested on:
- Chrome
- Firefox
- Safari
- Edge

**Why Different Browsers Matter:**
Different browsers may have:
- **Different Rendering Engines:** How they display graphics
- **Different Performance Characteristics:** Some are faster than others
- **Different JavaScript Implementations:** How they execute code

**Testing Requirements:**
For each browser:
- Test that the indicator loads and functions
- Verify rendering is correct (all visual elements appear properly)
- Verify performance is acceptable
- Verify no console errors occur
- Ensure all features work identically across browsers

**6. Data Availability Edge Cases Testing:**

**Test Scenarios with Limited Data:**
The indicator must handle these edge cases gracefully:
- **New Symbol with Limited History:** Symbol that just started trading
- **Symbol with Gaps in Data:** Missing price data between bars
- **Symbol with Extended Market Closure:** Long periods without trading
- **Symbol with Data Feed Interruption:** Temporary loss of data feed
- **Symbol with Irregular Trading Hours:** Non-standard trading schedule

**Testing Requirements:**
For each scenario:
- Test that the indicator handles the situation gracefully
- Verify it degrades functionality appropriately (shows partial data rather than crashing)
- Verify error messages are clear and helpful
- Verify no crashes or errors occur
- Ensure the indicator recovers when data becomes available again

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
