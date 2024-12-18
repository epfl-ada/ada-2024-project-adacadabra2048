# From Classroom to Screen: Exploring the Boom of Indian STEM Education on YouTube
# ADAcadabra2048 Project
> [Course Project](https://epfl-ada.github.io/teaching/fall2024/cs401/projects/) for [Applied Data Analysis 2024](https://epfl-ada.github.io/teaching/fall2024/cs401/), [EPFL](https://www.epfl.ch/en/)

> Team Name: ADAcadabra2048

> Project Members (sorted alphabetically by last name): Antonina Alekseeva, Ching-Chi Chou, Bich Ngoc (Rubi) Doan, Yasmine Kroknes-Gomez.
> 
## Abstract
In the maze of late-night study sessions and looming deadlines, many of us have turned to YouTube for a lifeline and often found it in the clear, concise explanations of Indian STEM tutors. This project explores how these creators have redefined digital learning, transforming YouTube into a global classroom. By analyzing YouTube metadata, including trends in video uploads, viewership, and engagement metrics, we trace the growth of Indian STEM content over time. Aiming to use sentiment analysis on comments, we want to uncover what resonates most with viewers, whether it be teaching styles or accessibility. To further enrich our understanding, we want to deploy advanced keyword classification based on hierarchical structures of STEM topics and measure content alignment through cosine similarity. From the heartfelt comments of grateful learners to the recurring communities built around these channels, this project tells the story of how Indian tutors are not just saving students—but shaping the future of inclusive, diverse, and empowering STEM education worldwide.

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

#### Note for Milestone 2:
To define whether the video is STEM-related or not, we tried to measure the cosine similarity between each of its tags and the names of categories. To encode tags and categories, we selected the model [all-MiniLM-L6-v2]( https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2), a light and low-dimension model which is easy to run locally. With a subset of the whole video dataset, we selected the cosine similarity threshold=0.5, which demonstrated mostly full and accurate results.

The code for this method is available in `/tests/stem_video_classification/`.

Although the subset results were convincing, we received a very disappointing categorization when filtered out all of the videos without country info, which is necessary for testing our hypothesis. The first iterations of this method were especially computationally heavy and time consuming, hence we couldn't test more models to see if this would allow us to qualify the rows properly. Comparing the models' quality and filtering will be implemented in Milestone 3.

## Methods
Note that some parts of each task were (partially) implemented in Milestone 2 to ensure their feasibility. We will carry out more detailed analysis, at the same time filter out the most important visualizations and findings that align well with our research questions later on in Milestone 3.


### Task 1: What makes Indian STEM tutorials special? (Framing the Trend of Rising Popularity among Indian STEM Content)
* **Descriptive Analysis**: Begin by describing the dataset with an emphasis on videos from Indian creators. This could reveal a possible growth pattern in Indian educational content. Examine:
    * **Basic statistics**: Total number of Indian STEM videos compared to STEM videos from other countries
    * **Visualize** trends in viewership, likes, dislikes, and subscriber counts.
* **Historical Context:**
    *  **Temporal Analysis**: Use upload dates to show how Indian STEM content has grown over time. Explore whether there’s a specific growth spurt, possibly around particular global events or shifts in online learning trends.

### Task 2: What drives Indian making educational content (Influence of Channel Popularity and Content Characteristics)

* **Influencer Impact:** 
    * **Basic statistics**: Use channel subscriber counts and the SocialBlade rankings to see if top-ranked Indian educational channels drive this trend. (i.e are the top-ranked channels Indians?)
    * Influencer impact could be discussed in country-scale, for example, big data, deep learning, chat GPT may initially emerge from in some developed countries, and then after some time lag, Indians start to make these tutorials 
        
* **Causal or Correlational Analysis**:
    * Use causal inference / correlation analyses to see if certain content characteristics—like video length, frequency of uploads, or type of tags—are linked to higher engagement or viewer retention on Indian STEM videos
    * This is to answer/connect to the previous question: If Indian is indeed the top-ranked STEM channel: Are there certain characteristics that make them successful? Or even if Indian is not on top, it’s also interesting because why do they still make videos, even when they’re not high-ranked!?
    * May have relation with levels of educations. Why many channels with less subscribers still making educational videos? Maybe many Indian cannot higher education so YouTube become a way
* **Viewership Patterns Analysis:** On the top-ranked Indian channels:
    * **Temporal analysis**: Use time-series data to track how views and subscribers have changed for channels posting Indian STEM content. This could reveal trends of increasing viewership or shifts in subscriber base around educational events or exam seasons.

### Task 3: Are Indian teachers good at STEM content? (Audience Engagement and Content Reception)
* **Engagement Metrics Analysis**: Look into the comments, likes, and dislikes to gauge audience reception. Analyze:
    * Whether Indian STEM videos have higher or lower engagement than non-STEM or non-Indian content. 
    * Disaggregate likes, dislikes, and comments per video to assess the audience's interest in Indian educational content
* **Sentiment Analysis / Text Analysis**: What are the comments talking about? Are they discuss about the course content or just praising the content creator?
    * Crawl some top comments of a sample of videos using Youtube API.
    * Conduct sentiment analysis on comments to identify overall viewer attitudes. For deeper insights, categorize/cluster sentiments by themes or common terms to see what aspects of content resonate most. (e.g some topics have more negative sentiment than others?)
    * Conducting Commenter (or channel) network analysis to see what kind of STEM videos are usually studied together (or made by a channel)

## Timeline and Organization
* **Step 1:** Refine and finish data preprocessing pipeline for classifying STEM Content *(Tonya & Yasmine)*
* **Step 2:** Implement task 1-3, splitting among team members. Choose the visualization and insights that fit the most with our overaching research goals; having meetings to confirm, give feedback and agree on chosen implementations. *(Rubi, Chingchi, Yasmine)*
* **Step 3:** Combine results and check for coherence and consistency with research questions. Construct the data story accordingly. *(Everyone)*
* **Step 4:** Develop data website and prepare the final report. *(TBD)*


## Questions for TAs (optional): 
* The STEM data filtering seems to be a big obstacle for us at the moment. We've tried to used different methods (simple regex, keyword crawling and filtering, converting to vector space etc), but they all seem to have its drawbacks, whether of computational resource consumption or accuracy. The one we chosen in the end seems to be the most "reasonable" one, however it still might fail to capture the majority of STEM content. Some of the classified STEM tags might not be relevant due to tag misuse or no tag used at all. Is it okay to leave it as a limitation, or should we spend more effort refining this preprocessing pipeline?


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
- Within the `channels` folder:
  - `education_channel_with_country.csv` - contains all channels within education category, added a "country" column
  - `other_channel_with_country.csv` - contains all channels within other (not education) category, added a "country" column
- Within the `videos` folder:
  - `videos_edu_with_country_nonan.csv` - contains all videos with education category, added a "country" column (no NaN)
  - `videos_stem.csv` - contains all videos with education category _and_ as classified as STEM, added a "country" column (no NaN)

