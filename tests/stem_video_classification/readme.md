## Motivation
To figure out if Indian people really contribute so much in STEM-related fields, we need to define STEM-related YouTube videos. One 
## How to run this code
0. Ask @trip1ech acces to data and place it to `.././data`
1. Prepare STEM tags in `stem_tags_preparation.ipynb` 
2. Having STEM tags, run `stem_video_detection.ipynb`
## Results
Unfortunately, the resulting `stem_videos.csv` dataset is far from containing STEM-only entries. It includes videos on politics, psychology, culture and entertainment content. We see the problem in:
- tags filtration process
- improper usage of tags (or not using tags at all)
## Challenge
For this version, we used a small language model [all-MiniLM-L6-v2]( https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) which is a limitation for achieving a coherent result. Calculating results using models with more parameters will allow us to evaluate how applicable this method to our challenge.
