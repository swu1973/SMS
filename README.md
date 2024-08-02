# SMS

Goal: Implement an AI model to efficiently gather a researcher's expertise based on their abstracts in the Astrophysics Data System (ADS). The AI model results will speed up the process of obtaining the best possible pool of proposal reviewers with relevant subject matter expertise. We input the names already in SMS, a database system used to create review panels, into the AI model and subsequently populate the expertise field with our results.


The LlamaModel is a continuation of Mallory Helfenbein's (NASA HQ intern 2023) ReviewerExtractor codeV2. You must create an ADS account and obtain an api token. We input a list of researcher names into the codeV2 which searches by first author in ADS, gather their abstracts from 2003 to 2030, and returns the top 10 words, bigrams, and trigrams (give example sheet of researcher names for the github). From these n-grams, we create a combined top words list.


We used the llama3-70b-8192 model in groqCloud. You must create a groqCloud account and obtain an api key. The llama model takes in the combined top words for each researcher and will determine the expertise chosen from the AAS keywords. We fed the model a specific prompt and the specific topics from AAS. First, the model is prompted to determine the general topics and then it is asked for their associated subtopics.


Files Needed:
- ADSsearcherpkg: Python file that has all of the functions used to find the expertises of the authors and produce an organized data frame with each row being an individual author and columns: 'Input Author','Input Institution', 'First Author', 'Bibcode', 'Title', 'Publication Date', 'Keywords', 'Affiliations', 'Abstract', 'Data Type'
- TextAnalysis.py: Python file that has all the functions in order to determine the top words, bigrams and trigrams in each publication.
stopwords.txt: Text file that has a list of the stop words for language processing.
- LlamaModel.ipynb: A notebook that contains the Llama model


LlamaModel Limitations:
- We are searching only by first author names and if the name is common the expertise may not be accurate 
- We are trusting the format of the llama model output but there may be cases where the format is different so manual work is required (Excel functions below)
- It takes about 4 minutes to run 10 researchers
- Rate limits in groqCloud: 30 requests per minute


Google Sheets Excel Functions for the Final Results:

Remove Duplicates: Data → Data Cleanup → Select these columns: Input Author, Affiliations, Combined Top Words, Number of Papers, and Model Expertise

Looking for “None” in the Subtopics column: =ISNUMBER(SEARCH("None",R2))
To Filter Columns: Data → Create Filter. Filter by TRUE
Do manual checks and edits. Refer to the “Subtopics with Explanation” column for model reasoning.

Looking for “not listed” in the Subtopics column: =ISNUMBER(SEARCH("- not listed",R2))
To Filter Columns: Data → Create Filter. Filter by TRUE.
Do manual checks and edits. Refer to the “Subtopics with Explanation” column for model reasoning. 

Remove redundant information. For example, you can delete “black holes:[‘not listed’]” if black holes are already listed as one of the subtopics.

Note: Some people may have an empty combined top words list because there are no abstract results. You can set these people aside.


SMS Search Results:
The model expertise is formatted as such: 
topic1:[‘subtopic 1’, ‘subtopic 2’, …]|topic2:[‘subtopic 1’, ‘subtopic 2’, …]|...|Number of Papers:#


For example,
physical data and processes:['radiation mechanisms: non-thermal', 'radiation mechanisms: general', 'waves', 'radiation belt dynamics', 'particle acceleration']|Sun:['particle emission', 'Sun: radiation belt', 'Sun: radiation']|Number of Papers: 32


Code Citation Credits:

- Máire Volz

- Mallory Helfenbein

- Isabelle Hoare

- Kaniyah Harris

- Antonino Cucchiara, PhD

- Sophia Wu

- Sofia Lendahl

