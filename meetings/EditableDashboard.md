# Implementation Steps - Manual Chart Configuration

## Step 1: Define Configuration Type System

**File:** `types.ts`

- Created `ChartConfiguration` interface as the main configuration model
- Defined 8 sub-interfaces for different chart aspects:
  - `AxisConfig` - axis labels, scales, ticks, min/max values, grid lines
  - `LegendConfig` - position, alignment, direction, sizing, symbol shape
  - `SeriesConfig` - line width/style, interpolation curves, area fill
  - `GridConfig` - visibility, colors, line styles
  - `PointConfig` - size, borders, colors, hover states
  - `TooltipConfig` - format, colors, styling
  - Plus layout and appearance configs
- Exported `DEFAULT_CHART_CONFIG` constant with sensible defaults matching current chart behavior

## Step 2: Build Configuration Panel Component

**File:** `ChartConfigPanel.tsx`

### Component Setup

Created `ChartConfigPanel` component accepting:
- `config: ChartConfiguration` - current configuration state
- `onConfigChange` - callback to update configuration
- `isManualMode` - toggle state
- `onToggleManualMode` - callback to enable/disable manual mode

### UI Components

Used Shadcn UI components:
- Sheet (side panel)
- Tabs
- Switch
- Input
- Select
- Label
- Button

### Helper Functions

Implemented helper functions:
- `updateConfig()` - updates top-level config sections
- `updateNestedConfig()` - updates nested properties (e.g., `axes.xAxis.label`)

### 3-Tab Interface

- **Layout Tab:** Margin inputs (top/right/bottom/left), legend visibility/position/direction
- **Axes Tab:** X-axis and Y-axis controls for labels, font size, tick count, min/max, grid lines
- **Style Tab:** Line width, interpolation type, area fill, point size/borders, grid styling

**Note:** All controls disabled when `isManualMode={false}`

## Step 3: Import Types and Panel in LineChart

**File:** `LineChart.tsx`

Added necessary imports for the configuration panel and types.

## Step 4: Add State Management to LineChart

- Created two state hooks for managing configuration
- Initialized `manualConfig` with defaults plus current axis labels from props
- Created `activeConfig` variable that switches between manual config and defaults based on `isManualMode`

## Step 5: Apply Configuration to Chart Props

- Modified chart margin calculation to use `activeConfig.layout.margin`
- Updated Nivo `chartProps` to use configuration values:
  - `enablePoints` from `activeConfig.points.enabled`
  - Y-axis scale min/max from `activeConfig.axes.yAxisLeft.min/max`
  - Conditional axis rendering based on enabled flags
  - Tick values from config (with fallback to defaults)
  - Label text, font size, offsets from axis configs
  - Point size, border width from point config
  - Line width, area fill from series config
  - Grid visibility from grid config

## Step 6: Add Configuration Panel to UI

- Positioned `<ChartConfigPanel>` component in top-right corner of chart
- Passed state and callbacks appropriately
- Made legend conditional on both `showLegend` prop AND `activeConfig.legend.enabled`

## Step 7: Type Safety & Testing

Fixed TypeScript errors:
- Removed unused React import
- Fixed curve interpolation types to match Nivo's `LineCurveFactoryId`
- Ensured min/max values properly typed as `number | "auto"`
- Verified type-checking passes (except unrelated PageCanvas errors)

---

## Result

Users can click "Chart Settings" button → toggle Manual Mode → adjust any chart property → see changes applied immediately to the chart visualization.

---

**Model:** Claude Sonnet 4.5 • 1x