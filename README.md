# Transcript Cleaning

The goal of this project was to take the web-scraped transcript of a podcast and organize it so that it could be used as input data for a transformer. More specifically, we wanted to look at each statement in the transcript, label who said it, what conversation it was said in (podcast episode), and the order in which it was said. This will enable us to use this data for a conversational agent built on a large-language model. 

spaCy was used for named entity recognition and BeautifulSoup was used as part of stripping hexcode and unicode characters from the text.

Our final data looks something like the table below:

| Index | Conversation | Speaker | Statement |
| ----- | ------------ | ------- | --------- |
| 0 | Episode 1 | Speaker 1 | Statement 1 |
| 1 | Episode 1 | Speaker 2 | Statement 2 |
| 2 | Episode 1 | Speaker 1 | Statement 3 |
| 3 | Episode 1 | Speaker 2 | Statement 4 |
| 4 | Episode 2 | Speaker 1 | Statement 5 |
| 5 | Episode 2 | Speaker 3 | Statement 6 |
| 6 | Episode 2 | Speaker 1 | Statement 7 |
| 7 | Episode 2 | Speaker 3 | Statement 8 |

This will make it easy to do some exploratory data analysis and set us up for ingestion into a conversational agent where the data needs to look like this:
| Index | Conversation | Speaker | Statement |
| ----- | ------------ | ------- | --------- |
| 0 | Conversation 1 | Speaker 1 | Question |
| 1 | Conversation 1 | Speaker 2 | Answer |
| 2 | Conversation 1 | Speaker 2 | Question |
| 3 | Conversation 1 | Speaker 1 | Answer |

The data was collected using web-scraping techniques and can be found in my other repository [Transcript Scraping](https://github.com/lindbergag/transcriptscraping)

## Requirements
* Python 3.6+
* Pandas
* spaCy
* Collections
* BeautifulSoup
* re
* warnings


## Setup
1. Clone the repository.
2. Install the required packages.
3. Edit code box 3 with the directory to your dataset.
4. Follow the steps that were taken in this project as a general guideline for cleaning your data.
5. Modify the code so that it's tailored to your data.
6. Run the script.
7. Check the outcomes.
8. Modify your script.
9. Run it again.
10. Repeat steps 4-9 until you're satisfied with the format of your data.


## Contributing

Feel free to fork the repository and submit pull requests with any improvements or bug fixes.

## Author's notes

If you want to take on a project like this one, expect it to be very time consuming and involve a lot of trial-and-error. Cleaning this data required a bespoke approach, and other similar ideas will most likely also require a bespoke approach. More advanced machine learning techniques may help speed up the process. 

For this particulary project, the way we initially scraped the data made labeling the episodes straight-forward, but labeling the speakers involved trying several strategies, and then applying parts of each to the end result. For this dataset, we used the following steps and strategies(and more):

1. A new line character, "\n", meant that the previous line was either a speaker or a statement.
2. we separated each line into its own row in our dataframe.
2. We collected a list of guests by using spaCy's named entity recognition on the episode titles.
3. For the titles that did not have a guest in our list, we manually added those speakers.
4. We used the number of unique speakers and number of episodes to get an idea of how many names we were missing.
5. We looked at lists of names that were outside of the expected word count (long and short) and compared those to the titles that had those names in them.
6. We manually added all of the names in those titles to our list of speakers (maybe creating some duplicates).
7. We then looked at the episodes where none of the names in our list of speakers occurred in the title, and manually added those names.

8. We used that list of speakers for an initial pass of our data, then filtered the episodes to those where there were fewere than 2 speakers, and episodes where there were more than 2 speakers. (flagging potentially missed speaker names, and potential issues).
9. We manually added the missing speaker names and recreated our dataframe with the new list.
10. We had to do some data cleaning because of mispelling names, and formatting errors. (For example, one episode didn't have a new line after a speaker's name)

11. We used Python's built in isTitle function to flag lines that were in a title format.
12. We used spaCy's named entity recognition to identify the names in the lines that were in a title format.

13. We compared our data to transcripts on the website to validate our labels and ensure we weren't using people's names that were intended to be a statement as potential speakers.

14. We manually combed through the lines formatted as a title to parse, modify, and correct names that weren't captured by spaCy's named entity recognition
15. We manually combed through the lines whose wordcount was less than our longest name (4) to parse, modify, and correct names that weren't captured in previous steps.

16. We concatenated the lines so that each row was an entire statement from 1 speaker, rather than 1 statement having several rows.
