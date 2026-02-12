# Women's Fashion & Accessory Product Dataset - Exploratory Data Analysis

## 📋 Project Overview

This project involves a comprehensive **Exploratory Data Analysis (EDA)** of a women's fashion and accessory product dataset containing 4,566 products across 7 categories and 57 brands. The analysis covers data exploration, cleaning, statistical analysis, and visualization through both Python and Power BI dashboards.

**Repository contains:**
- Python notebook with complete EDA workflow
- Cleaned dataset (CSV export)
- Power BI interactive dashboard
- Detailed documentation and insights

---

## 📊 Dataset Description

### Dataset Specifications
| Metric | Value |
|--------|-------|
| **Total Records** | 4,566 products |
| **Total Features** | 11 original columns |
| **Product Brands** | 57 unique brands |
| **Categories** | 7 product categories |
| **Date Range** | Current snapshot (no temporal data) |
| **Data Quality** | 99.7% complete (after cleaning) |

### Feature Overview

| Column | Type | Description | Significance |
|--------|------|-------------|--------------|
| **Product ID** | String | Unique identifier for each product | Primary key, data integrity |
| **Product Name** | String | Descriptive product name | Identifies specific products |
| **Brand Name** | String | Manufacturer/brand name | Market segmentation, brand analysis |
| **Product Description** | String | Detailed product specifications (color, style, material) | Product differentiation |
| **Category** | String (7 types) | Product category classification | Segmentation for analysis |
| **Product Size** | String | Available sizes (S/M/L or numeric) | Inventory, customer preferences |
| **MRP** | String | Maximum Retail Price (manufacturer suggested price) | Pricing strategy analysis |
| **Sell Price** | Numeric | Actual selling price | Revenue, margin analysis |
| **Discount** | String | Discount percentage offered | Promotional strategy |
| **Currency** | String | Currency type (all Rs.) | Standardization check |

### Product Categories

The dataset spans 7 distinct product categories with different pricing, sizing, and discount patterns:

```
1. Westernwear-Women      1,654 products (36.2%) - Largest segment
2. Indianwear-Women       1,131 products (24.8%) - Second largest
3. Footwear-Women           480 products (10.5%) - Shoes and footwear
4. Jewellery-Women          438 products (9.6%)  - Jewelry and accessories
5. Lingerie&Nightwear       367 products (8.0%)  - Intimate apparel
6. Watches-Women            329 products (7.2%)  - Premium segment
7. Fragrance-Women          167 products (3.7%)  - Luxury segment
```

**Category Characteristics:**
- **Westernwear & Indianwear**: Large variety, lower price points, varied discounts
- **Footwear & Apparel**: Standardized sizing, consistent pricing
- **Jewelry, Watches & Fragrance**: No size information (category-appropriate), varied pricing strategies

---

## 🔍 Part 1: Data Exploration

### Initial Data Assessment

**Objective:** Understand data structure, types, and distribution

#### Key Activities:
1. **Shape and Structure Analysis**
   - Dataset dimensions: 4,566 rows × 11 columns
   - All 4,566 products have unique IDs (no duplicates)

2. **Data Type Validation**
   - Numeric columns: S.No, SellPrice (integers)
   - String columns: BrandName, ProductID, ProductName, Description, Category, Size, Currency, MRP, Discount
   - **Finding:** MRP stored as string (will impact cleaning)

3. **Feature Distribution**
   - Product ID: 100% unique (perfect uniqueness)
   - Category: 7 distinct values (balanced across categories)
   - Size: 73.9% populated (category-dependent pattern)
   - Currency: Single value "Rs." (consistent)

4. **Initial Data Quality Observations**
   - **Missing Values:**
     - MRP: 13 null values (0.28%)
   
   - **Data Anomalies Detected:**
     - MRP column contains inconsistent values (0, 8.9, .9, decimals)

---

## 🧹 Part 2: Data Cleaning & Transformation

### Challenge 1: Missing & Corrupted MRP Values

**Problem:** 
- 1,256 corrupted MRP values (27.5%) including zeros, decimals < 100 (0, 8.9, .9, 4.5)
- 13 null values
- Total: ~1,269 invalid entries (27.8%)

**Root Cause Analysis:**
The corrupted values are NOT random errors but systematic patterns suggesting:
- Possible import/conversion errors from different data sources
- Different decimal formatting systems (.9 instead of 0.9)
- Zero values as placeholders for missing data

**Solution Implemented:**

Instead of discarding 27.8% of data, I **recovered the original MRP using the relationship between SellPrice and Discount:**

```
Original Formula: MRP = SellPrice / (1 - Discount%)

For a product with:
  - SellPrice: 1,500
  - Discount: 30%
  
Calculated MRP = 1,500 / (1 - 0.30) = 2,142.86
```

**Benefits of this approach:**
- ✅ Preserves all 4,566 products (no data loss)
- ✅ Maintains data integrity
- ✅ Creates accurate MRP based on actual pricing relationships
- ✅ Enables proper margin analysis

---

### Challenge 2: Column Naming Issues

**Problem:** 
- "Brand Desc" column contained product descriptions (colors, styles), not brand information

**Solution:**
- Renamed "Brand Desc" → "Product Description" (accurate semantic meaning)
- Improved readability and code maintainability

---

## 📈 Part 3: Statistical Analysis

### 3.1 Overall Dataset Statistics

**Pricing Metrics:**
```
Sell Price Analysis:
  Mean:        2,005.22
  Median:      1,379.00
  Std Dev:     2,259.61
  Min:         89.00
  Max:         25,995.00
```

**Key Insight:** High coefficient of variation (112.7%) indicates significant price diversity across product types. Distribution is right-skewed with more products at lower price points.

**Discount Metrics:**
```
Discount Percentage:
  Mean:        29.99%
  Median:      30.00%
  Mode:        10.00% (most frequent)
  Range:       5% - 80%
```

**Key Insight:** Bimodal distribution suggests two distinct promotional strategies - standard discounts (10%) and aggressive clearance (50%).

---

### 3.2 Category-Level Analysis

**Price Positioning by Category:**

```
Premium Positioning (High Price, Low Discount):
├─ Watches-Women      7,371 avg | 21.2% avg discount
└─ Fragrance-Women    3,065 avg | 15.2% avg discount

Mid-Range (Moderate Price & Discount):
├─ Footwear-Women     2,583 avg | 28.7% avg discount
└─ Indianwear-Women   1,855 avg | 25.0% avg discount

Value Positioning (Low Price, High Discount):
├─ Westernwear-Women  1,444 avg | 32.9% avg discount
├─ Lingerie&Nightwear 649 avg  | 36.3% avg discount
└─ Jewellery-Women    582 avg  | 40.2% avg discount
```

**Strategic Observations:**
1. **Price-Discount Inverse Relationship**: Premium categories (Watches, Fragrance) use lower discounts to maintain brand perception
2. **Category Margins**: Watches have 8-10x higher pricing than Jewelry despite similar discount strategies
3. **Volume vs. Margin Trade-off**: Apparel categories (61% of catalog) prioritize volume with moderate discounts

---

### 3.3 Brand Analysis

**Market Concentration:**
```
Top 10 Brands: 68.8% of products
Top 5 Brands:  47.3% of products
Top Brand:     "and" with 709 products (15.5% share)

Total Brand Count: 57 (healthy diversity)
Avg Products/Brand: 80 products
```

**Brand Portfolio by Strategy:**

| Ranking | Brand | Products | Avg Price | Positioning |
|---------|-------|----------|-----------|--------------|
| 1 | and | 709 | 1,717 | Volume leader, mainstream |
| 2 | aurelia | 486 | 1,461 | Value-focused |
| 3 | ayesha | 381 | 491 | Ultra-budget |
| 4 | biba | 290 | 2,218 | Premium-mid |
| 5 | adidas | 248 | 2,765 | Sports/premium |
| 6 | casio | 198 | 5,606 | Ultra-premium (watches) |

---

### 3.4 Margin & Pricing Analysis

**MRP vs. Sell Price Comparison:**
```
Recovered MRP Analysis:
  Average MRP:           2,422
  Average Sell Price:    2,005
  Average Profit Margin: 20.5%
  Median Margin:         30.0%
```

**Margin Distribution by Category:**
```
Watches:           15-25% margin (premium, less discount)
Fragrance:         20-30% margin (brand protection)
Footwear:          25-35% margin (competitive)
Apparel:           30-40% margin (volume with discounts)
Jewelry:           40-50% margin (high discount strategy)
```

---
