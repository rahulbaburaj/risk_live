# News Processing Server README

## Overview
This server automates the enhancement of news data by tagging each article with LLM-generated summaries, relevance scores, alert flags, and other relevant metadata. It is designed to run whenever there is an update in the source data files, ensuring that the latest news articles are enriched with valuable insights.

## Features
- **Automated Data Processing**: Automatically detects new or updated news articles for processing.
- **LLM Integration**: Utilizes Azure OpenAI to generate summaries, relevance scores, alert flags, and keywords for each news article.
- **Data Cleaning**: Cleans the dataset by removing unwanted summaries and duplicates.
- **Continuous Monitoring**: Uses a scheduling mechanism to check for new data at regular intervals and processes it accordingly.
- **Logging**: Comprehensive logging of the processing steps and errors for debugging and monitoring purposes.

## Tagging Process and Example
The core functionality of this server lies in its ability to tag each news article with metadata generated by OpenAI's GPT-4, enhancing the dataset with valuable insights. Below is an explanation of the tagging process and an example of the output.

### Tagging Process
For each article, the server:
1. Extracts the summary of the article.
2. Creates a prompt that includes the article summary and sends it to Azure OpenAI.
3. Receives a response from GPT-4, which includes a set of tags comprising keywords, a short summary, relevance to a certain domain, and an alert flag with its reason.

### Example of Tagged News
Consider an article discussing the challenges in maternity services. The tagging process would yield the following metadata:

```json
{
  "RelevantKeywords": ["midwives", "maternity services", "staffing levels", "induction of labour", "health and safety"],
  "ShortSummary": "The article discusses the challenges faced by midwives in maternity services, including staffing shortages, limited aftercare, and traumatic experiences for women giving birth. It highlights the impact of these issues on the safety and well-being of both mothers and babies.",
  "Relevance": "Yes",
  "RelevanceReason": "The article is relevant to the NDA as it addresses health and safety risks in maternity services, specifically related to staffing levels, which can impact the quality of care provided to mothers and babies.",
  "AlertFlag": "Red",
  "AlertReason": "The article is of high importance as it highlights significant risks and challenges in maternity services, which can have a direct impact on the well-being and safety of individuals. It raises concerns about the sustainability of maternity services and the need for urgent attention to address the issues faced by midwives."
}
```

This metadata enriches the dataset by providing a concise summary, identifying relevant keywords, assessing the article's relevance, and flagging any urgent issues. Such tagging facilitates a deeper understanding of the news content and its implications.

## Requirements
- Python 3.x
- Pandas library
- Dotenv library for environment variable management
- OpenAI library for Azure OpenAI integration
- Schedule library for task scheduling

## Setup
1. Clone the repository to your local machine.
2. Install the required Python libraries: `pip install pandas python-dotenv openai schedule`.
3. Create a `.env` file in the root directory and add your Azure OpenAI credentials and any other necessary configuration variables.
4. Ensure you have the initial news data CSV file (`newscatcher_df.csv`) in the `./data` directory.

## Usage
To start the server and begin processing:
1. Navigate to the server's directory.
2. Run the script: `python topicgpt.py`.
3. The script will continuously monitor for new or updated news data files and process them as needed.

The server checks for new updates every minute and processes any new articles found by tagging them with LLM-generated metadata. Processed articles are saved in a new CSV file (`newscatcher_df_with_response.csv`) in the `./data` directory, enriched with the generated metadata.

## How It Works
1. **Data Monitoring**: The server checks for new data by comparing the existing processed dataset with the latest dataset.
2. **Data Processing**: For each new or updated article, the server generates a prompt based on the article summary and sends it to Azure OpenAI. The response is parsed to extract relevant metadata, which is then tagged to the article.
3. **Saving Processed Data**: The enriched dataset is saved, ensuring that no duplicates are created in the process.

## Customization
- Modify the `prompt.txt` and `response_format.txt` files in the `./prompt` directory to change the input prompt and response format for the Azure OpenAI calls.
- Adjust the scheduling frequency in the script according to your requirements.


