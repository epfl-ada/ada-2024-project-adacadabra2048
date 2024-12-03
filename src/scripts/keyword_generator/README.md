# Keyword Extraction Process

This code extracts and filters lecture video keywords from the MIT OpenCourseWare (OCW) site. The workflow integrates scraping, filtering, and organizing data into a nested hierarchical structure. It also refines keywords for accurate categorization and analysis. The goal is to ensure a clean, organized, and deduplicated dataset for STEM-related lecture videos, ready for further analysis and keyword-based categorization.

# Keyword Extraction Process

## Steps in Keyword Extraction

### 1. **Web Scraping Setup**
- The scraper uses Selenium to automate browsing, accessing STEM lecture video data from online sources.
- Configured to explore categories such as "Science," "Engineering," "Mathematics," and "Computer Science."

---

### 2. **Data Collection**
- Each topic is scraped, and its associated course and lecture titles are collected.
- Infinite scrolling and lazy-loading mechanisms ensure a comprehensive data extraction.

---

### 3. **Initial Data Cleaning**
- Titles and course names are stripped of irrelevant phrases, punctuation, and parenthetical content (e.g., "(cont.)", "Lecture 1:").
- Keywords are split into individual components for granular processing.

---

### 4. **Reorganizing JSON**
The `reorganize_json` function creates a well-defined hierarchical structure for the data:
- **Primary Categories**: "Science," "Engineering," "Mathematics," and "Computer Science."
- **Secondary Categories**: Nested categories such as "Physics," "Chemistry," and "Electrical Engineering."
- **Tertiary Categories**: Further subcategorization, e.g., "Quantum Mechanics" under "Physics."

#### **Key Enhancements:**
1. **Hierarchical Nesting**: Subcategories are nested within relevant parent categories.
2. **Category Deduplication**: Duplicate entries across parent and subcategories are removed.
3. **Consistent Naming**: Category-specific data is renamed to match its parent for clarity.

---

### 5. **Keyword extraction**
- A unique set of keywords is extracted from the general category of each primary STEM field for exploratory analysis.
- These lists are output in a plain text format for review.

---

### 6. **Final Output**
Processed files include:
- **`final_stem_lectures.json`**: A clean, reorganized hierarchical dataset.
- **`cleaned_keywords_per_category.txt`**: Keywords sorted and categorized for STEM fields.

---

## Dependencies

### Python Packages:
- **Selenium**: For automated web scraping.
- **JSON**: (Standard library) For data handling.
- **re and NLTK**: For text cleaning and keyword filtering.
---

### Hierarchical Structure

#### Primary Categories:
- **Science**
- **Engineering**
- **Mathematics**
- **Computer Science**

#### Secondary and Tertiary Subcategories:
- **Physics**
  - Atomic, Molecular, Optical Physics
  - Theoretical Physics
  - Quantum Mechanics
  - Nuclear Physics
- **Chemistry**
  - Organic Chemistry
  - Physical Chemistry
- **Electrical Engineering**
  - Signal Processing
  - Telecommunications

Refer to `final_stem_lectures.json` for the complete hierarchical structure.
