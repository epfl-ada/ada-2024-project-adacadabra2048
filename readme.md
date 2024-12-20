# From Classroom to Screen: Exploring the Boom of Indian STEM Education on YouTube
# ADAcadabra2048 Project
> [Course Project](https://epfl-ada.github.io/teaching/fall2024/cs401/projects/) for [Applied Data Analysis 2024](https://epfl-ada.github.io/teaching/fall2024/cs401/), [EPFL](https://www.epfl.ch/en/)

> Team Name: ADAcadabra2048

> Project Members (sorted alphabetically by last name): Antonina Alekseeva, Ching-Chi Chou, Bich Ngoc (Rubi) Doan, Yasmine Kroknes-Gomez.
> 
## Abstract
In the maze of late-night study sessions and looming deadlines, many of us have turned to YouTube for a lifeline—often finding it in the clear, concise explanations of Indian STEM tutors. This project seeks to uncover the factors that have propelled Indian creators to the forefront of STEM education on YouTube, exploring how their content evolved over time and comparing their strategies and engagement to those of other countries. By analyzing a rich tapestry of metadata, from geographic associations to monetization patterns, we investigate what sets Indian STEM channels apart: Are they uniquely positioned to grow their audience, or do their teaching styles and topic choices appeal more broadly? Integrating additional resources like the YouTube Data API, we delve into how comments form communities of recurring viewers and how learners navigate diverse STEM topics. From quantifying their rise to understanding their lasting impact on student engagement and sentiment, this project paints a vibrant picture of how Indian creators are not just helping viewers pass exams—they’re reshaping the landscape of inclusive, accessible STEM learning worldwide.

### Link to website:
[Final project website](https://ngoccc.github.io/ADAcadabra2048/)

## Research Questions: 
* How have Indian creators shaped the trend of STEM content on YouTube? What factors contribute to its popularity over time?
* What are the characteristics of top-ranked Indian STEM channels, compared to those of other countries?
* How does audience reception vary across Indian STEM videos? And what sentiments are most commonly expressed in the comments?
* Does Indian STEM content on YouTube foster recurring viewer communities? And how do viewers navigate the different sets of topics within this content?

## Proposed additional resources (if any): 
* **YouTube Data API**: Using the video and channel IDs in the dataset, we can fetch additional attributes that will valuable to our analysis, including:

    1. Video's `Country` - The country with which the channel (who posted the video) is associated. This allows us to extract geographic locations of the videos and channels, through which we can explore insights on the global trend of STEM content, comparing different engagement metrics between Indian and other regions in the world.
    2. Video's `CommentThreads` - Information about a YouTube comment thread, which comprises a top-level comment and replies, if any exist, to that comment. This data gives us rich insights into how viewers interact with video content through discussion in the comment section, and this pattern can further be extended for analysis across different videos by matching the commenters' id.

* **MIT OpenCourseWare (OCW)**: Offers free educational resources like lecture notes, videos, and assignments, organized by topics. We used its predefined topics as categories to build a hierarchical structure of STEM-related keywords, forming the foundation for analyzing STEM content in other datasets. 

## Data Preprocessing Pipeline (WIP)

![ada](https://hackmd.io/_uploads/Bk6m3VSfkx.png)

We used Selenium and Chromedriver to scrape lecture video data from MIT OpenCourseWare (OCW). Our pipeline extracted course and lecture titles across 152 predefined topics (e.g., Engineering, Theoretical Physics) of differing levels of specialisation and processed them into clean, usable keywords. This was done by the following steps:

1. **Web scraping:** Automatic navigation and infinite scrolling on OCW using Selenium, targeting video-related elements and collecting course and lecture titles. This was then saved as `lecture_videos_data.json`
2. **Data Cleaning and Keyword Refinement:** Removed unnecessary phrases (e.g., "Introduction to"), course codes, punctuation, and stopwords. Split lecture titles into distinct, concise keywords. Filtered duplicates and incomplete entries (e.g., "(cont.)").
3. **Hierarchical Organization:**
    **Initial Hierarchical Processing and Deduplication** - Removed duplicate lectures within each category using course and lecture titles as unique keys. Identified overlapping categories (≥90% overlap) and nested them under parent categories. Removed duplicates from parent categories already present in subcategories.
    
    **Main and Subcategory Nesting** - We defined the main categories (Science, Computer Science (as a proxy for Technology), Engineering, and Mathematics) to fit the STEM format. Existing subcategories were assigned to their respective main categories, ensuring they aligned with the STEM structure. Subcategories without lecture items were filtered out to maintain relevance. Each main category was populated with its filtered subcategories, forming a clean and logical hierarchical structure.

### Defining what a STEM video is:
Using our generated and cleaned keyword list from OCW, we proceeded to implement a pipeline to classify whether an educational video was STEM or not. We used FlashText to quickly scan titles and tags of **1.9 million educational videos** for exact STEM keyword matches defined by our 152 predefined topics. This was done by the following steps:

1. **Direct Keyword Matching (FlashText):** Instantly identified exact single-word and multi-word STEM keywords in titles and tags. If no direct matches were found, we proceeded to fuzzy matching.

2. **Fuzzy Matching for Multi-Word Phrases:** Generated up to 4-word n-grams from titles and tags to catch near-perfect (≥90% similarity) variants of STEM keywords (e.g., “Pyhton” vs. “Python”).

3. **Parallel Processing for Efficiency:** Applied multithreading (ThreadPoolExecutor) to handle large-scale text matching, speeding up the classification process across millions of videos.

4. **Tag Thresholding and Intersection Rule:** Ensured that at least half of a video’s tags matched STEM keywords and that its title also contained a STEM keyword, creating a strict two-tier verification to minimize false positives and negatives.

Our pipeline successfully filtered down millions of educational videos to approximately **74,717 STEM videos**. 

## Methods
Note that some parts of each task were (partially) implemented in Milestone 2 to ensure their feasibility. We will carry out more detailed analysis, at the same time filter out the most important visualizations and findings that align well with our research questions later on in Milestone 3.


### Task 1: What makes Indian STEM tutorials special? (Framing the Trend of Rising Popularity among Indian STEM Content)
* **Descriptive Analysis**: Begin by describing the dataset with an emphasis on videos from Indian creators. This could reveal a possible growth pattern in Indian educational content. Examine:
    * **Basic statistics**: Total number of Indian STEM videos compared to STEM videos from other countries
    * **Visualize** trends in viewership, likes, dislikes, and subscriber counts.
* **Historical Context:**
    *  **Topic Breakdown & Temporal Analysis**: Use upload dates to show how Indian STEM content has grown over time. Explore whether there’s a specific growth spurt, possibly around particular global events or shifts in online learning trends.

### Task 2: What drives Indian making educational content (Influence of Channel Popularity and Content Characteristics)

* **Money Matters: Are They Teaching for Profit?**:
    * To understand how Indian STEM content creators make profits from Youtube channels
* **Success Strategies: How Indian Channels Grow**:
    * Use correlation analyses to see if certain content characteristics—like frequency of uploads linked to higher engagement on Indian STEM videos
* **Exam Season Heroes: When Students Need Them Most:**
    * Examine whether there's a correlation between viewer's engaments and exam periods.
### Task 3: Quality Check: How Well Do Indian Tutors Teach? (Audience Engagement and Content Reception)
* **By the Numbers: How Indian Tutors Compare Globally**: Look into the comments, likes, and dislikes to gauge audience reception. Analyze:
    * Whether Indian STEM videos have higher or lower engagement than non-STEM or non-Indian content. 
    * Disaggregate likes, dislikes, and comments per video to assess the audience's interest in Indian educational content
* **The Verdict: What Students Really Think**: What are the comments talking about? Are they discuss about the course content or just praising the content creator?
    * Crawl some top comments of a sample of videos using Youtube API.
    * Conduct sentiment analysis on comments to identify overall viewer attitudes. For deeper insights, categorize/cluster sentiments by themes or common terms to see what aspects of content resonate most. (e.g some topics have more negative sentiment than others?)
    * Conducting Commenter (or channel) network analysis to see what kind of STEM videos are usually studied together (or made by a channel)

## Timeline and Organization
* **Step 1:** Refine and finish data preprocessing pipeline for classifying STEM Content *(Tonya & Yasmine)*
* **Step 2:** Implement task 1-3, splitting among team members. Choose the visualization and insights that fit the most with our overaching research goals; having meetings to confirm, give feedback and agree on chosen implementations. *(Rubi, Chingchi, Yasmine)*
* **Step 3:** Combine results and check for coherence and consistency with research questions. Construct the data story accordingly. *(Everyone)*
* **Step 4:** Develop data website and prepare the final report. *(TBD)*

## Project Structure

The directory structure of new project looks like this:

```
├── data                        <- Data for and from the preprocessing pipeline
│
├── src                         <- Source code
│   ├── data                            <- Keywords for classifcation on STEM-content
│   ├── scripts                         <- Scripts for data processing
│
├── tests                       <- Tests of classification on STEM-content
│
├── results.ipynb               <- The final results of Project Milestone 2 
│
├── .gitignore                  <- List of files ignored by git
│
└── README.md
```


## Data Access
The necessary files are hosted in a shared Google Drive folder, accessible at the following link:

[Google Drive: ADA dataset](https://drive.google.com/drive/folders/1yV7g05QrUFoErd6uBBaWjHgLU_RXLuBU?usp=sharing)

- `final_keywords.txt` - contains the list of keywords used for classifying STEM content
- `monetization_labels.json` - contains the dictionary of keyword-monetization strategies - taken from [this prior work](https://github.com/vegetable68/YouTube-Alternative-Monetization/blob/main/domains_dict_labels_final.jsonl)
- Within the `channels` folder:
  - `education_channel_with_country.csv` - contains all channels within education category, added a "country" column
  - `other_channel_with_country.csv` - contains all channels within other (not education) category, added a "country" column
  - df_timeseries_en.tsv.gz - raw dataset for channel time series data
- Within the `videos` folder:
  - `videos_edu_with_country_nonan.csv` - contains all videos with education category, added a "country" column (no NaN)
  - `videos_stem_with_descriptions.csv` - contains all videos with education category and is STEM, with description added (previously we did not include for analysis due to the size)
  - `videos_stem.csv` - contains all videos with education category and is STEM (without descriptions)
  - `videos_stem.csv` - contains all videos with education category _and_ as classified as STEM, added a "country" column (no NaN)

