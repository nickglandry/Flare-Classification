# Flare Classification using LLM Vision Tools

This repository contains **documentation, results, and example outputs** from my involvement in research of high temperature source classification. **I encourage you to view my [reasearch poster](Poster.pdf)** from the Colorado School of Mines Computer Science Spring 2025 Innovation Fair.

⚠️ Note: The full implementation code is **not publicly available**

# Abstract
This program intends to bridge the gap between detections of near realtime high temperature sources and visual context of the source. VIIRS satellite data gathers large amounts of useful metadata, but it does not capture the visual context of the surrounding environment. Thus, determining the source of high temperature detections is challenging.

To aid in this task, the program gathers high-resolution imagery of each flare site. The imagery is fed into a Large Language Model (LLM) with vision capabilities. For each flare, the model is prompted to return if the flare is of the desired type (ex: upstream) or is not. For flares that are of the desired type (ex: upstream), a recommendation value is assigned. For flares that are not of the desired type (ex: not upstream), the model prompt is changed to a new type (ex: downstream) and these remaining flares are given to the model for classification. This process continues until all known flare types are asked. Any remaining flares are tagged "unknown". 

# LLM Integration
Model: Gemini 2.0 flash
Instructions for creating a Gemini API Key can be found in the [Google AI for Developers Documentation](https://ai.google.dev/gemini-api/docs/api-key). An API Key for use on the Gemini 2.0 flash model can be created for free without entering a payment method.

# Additional Necessary API Keys
Google Maps Static API (used for fetching high-resolution satellite images). This is likely to be the most difficult part of the setup process; do not hesitate to reach out with questions.
Instructions for creating an API key can be found in the [Maps Static API Documentation](https://developers.google.com/maps/documentation/maps-static/overview). At the time of writing the README, Google Cloud Console allowed users to recieve $300 in free credits, which should be enough to install images for this project. Additionally, a certain number of images per day may be free (not counted against the credits). In order to generate an API key, you must add payment information to Google Cloud Console.

# Compilation/Execution Instructions
The file structure below shows the programs expectations for file management. Be sure to create the necessary folders and files to avoid any errors. All files in the \input_data\datasets folder must contain a dataset_type column, lat column, and lon column.

After creating your Python Virtual Enviornment (Python 3.13.3 suggested), use 'requirements.txt' to install all necessary dependencies.

You will first need to get all the images for the classification process. This can be done using 'getImages.py' and will store the images in 'input_data/images' as expected by the classification program.

After saving all necessary images, run the command `python main.py input\file\path -o output\file\path --metadata --verbose'. 
If the code ever crashes, you can re-run the same command. Don't worry, your progress is saved in the output csv and classified flares will not be re-classified.

After the program is finished, the output data CSV will contain flare indices and the classification result (ex: upstream or not_upstream). You can use 'filterResults.csv' to create a catalog with the desired flares.  

# Intended File Structure
```
└── flare_classification
    ├── input_data
    │   ├── datasets
    │   │    ├── fractracker_compression_stations_us.csv
    │   │    .
    │   │    .
    │   │    .
    │   │    └── Global_Oil_and_Gas_Plant_Tracker.csv
    │   ├── catalogs
    │   │    ├── new_in_MYC25.csv
    │   │    ├── new_in_MYC25_notUpstream.csv
    │   │    .
    │   │    .
    │   │    .
    │   │    └── new_in_MYC25_notSawmill.csv
    │   ├── images
    │   │    ├── image1
    │   │    │    ├── image1_radius001.png
    │   │    │    ├── image1_radius005.png
    │   │    │    └── image1_radius0025.png
    │   │    .
    │   │    .
    │   │    .
    │   │    └──  image999
    │   │    │    ├── image999_radius001.png
    │   │    │    ├── image999_radius005.png
    │   │    │    └── image999_radius0025.png
    │   ├── GEMINI_API_KEY.txt
    │   └── GOOLGE_MAPS_STATIC_API_KEY.txt
    ├── output_data
    │   ├── upstream_classification_results.csv
    │   .
    │   .
    │   .
    │   └── sawmill_classification_results.csv
    ├── dataMatch.py
    ├── getImages.py
    ├── LLMclassify.py
    ├── main.py
    ├── README.md
    └── requirements.txt
```

# Notes about future updates
The code does not automatically update prompts or filter beyond a single type (ex: upstream) without user input. If I had more time, I would implement these features. 
Prompts can be manually updated in 'LLMClassify.py'.
Filtering out positive classifications is currently done manually in excel (sorting by col and removing relevant flares).

# Contact Information
Please reach out to me via email with any questions or suggestions. 

nglandry@mines.edu (University)

nickglandry@gmail.com (Personal)
