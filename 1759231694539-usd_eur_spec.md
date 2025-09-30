# USD/EUR Exchange Rate Web App - Software Specification

## 1. Project Overview

**Application Name:** USD/EUR Exchange Rate  
**Description:** A lightweight web application for visualizing USD to EUR exchange rate data from CSV files  
**Technology Stack:** Node.js, Vite, Chart.js  
**Target Users:** Financial analysts, traders, or anyone needing to visualize USD/EUR exchange rate trends  

## 2. Functional Requirements

### 2.1 Core Features

#### 2.1.1 CSV File Upload
- Users can upload a single CSV file containing USD/EUR exchange rate data
- File size limit: 5MB maximum
- Supported format: .csv files only
- Single file replacement (new upload replaces previous data)

#### 2.1.2 Data Processing
- Parse uploaded CSV file automatically
- Remove rows with missing values (empty cells, null, undefined)
- Basic data validation:
  - Verify date format is valid
  - Ensure exchange rate is a positive number
  - Remove duplicate dates (keep most recent entry)
  - Sort data chronologically

#### 2.1.3 Interactive Line Chart
- Display exchange rate over time as a line chart
- X-axis: Date (chronological)
- Y-axis: Exchange rate (USD per EUR)
- Interactive tooltip showing:
  - Exact date
  - Exchange rate value
- Vertical crosshair on hover
- Smooth line interpolation between data points

#### 2.1.4 Time Range Selector
- Predefined time intervals calculated from today's date:
  - **1 Year**: Show data from past 365 days
  - **5 Years**: Show data from past 5 years (1,825 days)
  - **10 Years**: Show data from past 10 years (3,650 days)
  - **Max**: Show all available data
- Default selection: Max (show all data)
- Buttons/tabs for easy switching between intervals

#### 2.1.5 Chart Labels and Legend
- X-axis label: "Date"
- Y-axis label: "USD per EUR"
- Chart title: "USD/EUR Exchange Rate"
- Legend showing data series name: "Exchange Rate"

## 3. Technical Requirements

### 3.1 Frontend Framework
- **Build Tool:** Vite for fast development and building
- **JavaScript:** ES6+ modules
- **CSS:** Modern CSS with Flexbox/Grid for layout

### 3.2 Charting Library
- **Chart.js** (latest stable version)
- Required plugins:
  - Chart.js core
  - Line chart controller
  - Time scale adapter (for date handling)
  - Tooltip plugin
  - Legend plugin

### 3.3 CSV Processing
- **Library:** Papa Parse (lightweight CSV parser)
- Client-side parsing for simplicity
- No server-side processing required

### 3.4 File Upload
- HTML5 file input with accept=".csv"
- FileReader API for client-side file processing
- Basic file type validation

## 4. Data Format Specification

### 4.1 Expected CSV Structure
```csv
date,rate
2024-01-01,1.1045
2024-01-02,1.1032
2024-01-03,1.1058
...
```

### 4.2 Data Requirements
- **Date Column:** ISO date format (YYYY-MM-DD) preferred, but flexible parsing
- **Rate Column:** Decimal number representing USD per 1 EUR
- **Headers:** First row should contain column names
- **Encoding:** UTF-8

### 4.3 Data Validation Rules
- Dates must be valid and parseable
- Exchange rates must be positive numbers between 0.5 and 2.0 (reasonable bounds)
- Rows with missing date or rate values are dropped
- Future dates (beyond today) are filtered out

## 5. User Interface Requirements

### 5.1 Layout Structure
```
┌─────────────────────────────────────┐
│           App Header                │
│        USD/EUR Exchange Rate        │
├─────────────────────────────────────┤
│  [Choose File] [Upload Button]      │
├─────────────────────────────────────┤
│  [1Y] [5Y] [10Y] [Max] ← Time Range │
├─────────────────────────────────────┤
│                                     │
│           Chart Area                │
│         (Line Chart)                │
│                                     │
└─────────────────────────────────────┘
```

### 5.2 Color Scheme
- **Primary:** Various shades of blue (#007bff, #0056b3, #e3f2fd)
- **Background:** White (#ffffff)
- **Text:** Dark gray (#333333)
- **Chart Line:** Blue (#007bff)
- **Hover Effects:** Light blue highlights

### 5.3 Responsive Design
- Mobile-friendly layout (min-width: 320px)
- Chart resizes based on container
- Touch-friendly interface for mobile devices

## 6. Performance Requirements

### 6.1 File Processing
- CSV parsing should complete within 2 seconds for files up to 5MB
- Chart rendering should be smooth for datasets up to 10,000 data points
- Time range filtering should be instant (< 100ms)

### 6.2 Browser Compatibility
- Modern browsers supporting ES6+
- Chrome 80+, Firefox 75+, Safari 13+, Edge 80+

## 7. Error Handling

### 7.1 File Upload Errors
- Invalid file type: "Please upload a CSV file"
- File too large: "File size must be under 5MB"
- Empty file: "The uploaded file is empty"

### 7.2 Data Processing Errors
- No valid data after filtering: "No valid data found in the file"
- Invalid date format: "Invalid date format detected"
- Invalid exchange rate values: "Invalid exchange rate values detected"

### 7.3 User Feedback
- Loading indicator during file processing
- Success message after successful upload
- Error messages displayed in red text below upload area

## 8. Development Structure

### 8.1 Project Directory
```
usd-eur-app/
├── src/
│   ├── main.js           # Application entry point
│   ├── chart.js          # Chart.js configuration and management
│   ├── dataProcessor.js  # CSV parsing and data validation
│   └── styles.css        # Application styles
├── public/
│   └── index.html        # Main HTML template
├── package.json          # Dependencies and scripts
└── vite.config.js        # Vite configuration
```

### 8.2 Key Dependencies
```json
{
  "dependencies": {
    "chart.js": "^4.4.0",
    "chartjs-adapter-date-fns": "^3.0.0",
    "date-fns": "^2.30.0",
    "papaparse": "^5.4.1"
  },
  "devDependencies": {
    "vite": "^5.0.0"
  }
}
```

## 9. Implementation Priority

### Phase 1 (MVP)
1. Basic file upload functionality
2. CSV parsing with Papa Parse
3. Simple line chart with Chart.js
4. Basic time range selector

### Phase 2 (Enhancements)
1. Interactive tooltip with crosshair
2. Data validation and error handling
3. Responsive design improvements
4. Polish UI and styling

## 10. Testing Requirements

### 10.1 Test Cases
- Upload valid CSV files with various date formats
- Upload invalid files (wrong format, corrupted data)
- Test time range filtering with different datasets
- Verify chart interactivity (hover, tooltip)
- Test responsive behavior on different screen sizes

### 10.2 Sample Data
Create test CSV files with:
- Small dataset (30 days)
- Medium dataset (1 year)
- Large dataset (10+ years)
- Edge cases (missing values, invalid dates)

## 11. Deployment

### 11.1 Build Process
- Use Vite build command to generate production bundle
- Minified CSS and JavaScript
- Optimized for static hosting

### 11.2 Hosting Options
- Static hosting (Netlify, Vercel, GitHub Pages)
- No backend server required
- CDN distribution for Chart.js and dependencies

---

**Document Version:** 1.0  
**Last Updated:** September 19, 2025  
**Author:** Martin