# Sentiment Analysis on Tweets of Companies

Sentiment Analysis has become a widely discussed topic in the field of Natural Language Processing. The increasing volume of text-based data allows for opportunities to draw insights from the attitudes expressed. Some applications of sentiment analysis include: 

- analyzing customer reviews so companies can respond accordingly, 
- employee feedback to help improve employee engagement and wellbeing
- social media monitoring

As social media is ubiquitous in the 21st century, I have built a sentiment analysis project to analyze tweets. Twitter is one of the biggest digital platforms for users to connect with one another. Virtually any topic, commercial or otherwise, is shared on Twitter. Whatever was written gets quickly spread online.

Today there is a wealth of content to review for sentiment analysis. Organizations including startups, large companies, non-profits, and sole proprietors are finding ways to monitor their brand online. Therefore it is critical for companies to keep of reviews and any other social mentions. However, any curious user can find any opinion about anything online other than brands or products or services.

The objective is simple: to determine if a given tweet about a topic (brand, product, or otherwise) is positive, negative, or neutral. I will elaborate below.

Here is the data app, deployed to Heroku: https://analyze-tweets-proj.herokuapp.com

You can find the repository [here](https://github.com/VincentLu91/Tweet-Analyzer-by-Topic).

## Implementation

The data application was written in Flask and the text processing libraries used were Textblob and VADER. I chose Flask because it allows me to specify where and how to display the UI for the end user to perform sentiment analysis. Textblob and VADER are popular sentiment packages that work out-of-the-box and I like to use them to compare their sentiment results when processing a piece of text.

For data scraping, I used the [GetOldTweets3](https://pypi.org/project/GetOldTweets3/) Python Library. It enables the app to query tweets by topic name, username, dates, and any number of posts. The application currently queries 10 tweets by each topic name being processed.

As well, I used the Python ['translate'](https://pypi.org/project/translate/) and [langdetect](https://pypi.org/project/langdetect/) libraries to assess the language of the source tweet before translating to English for sentiment processing. However, the sourced tweets are displayed in their original language with the sentiments returned.

As well, the form was implemented with Flask request forms module for handling user input and error validation.

### Textblob

Textblob computes polarity and subjectivity scores for a text. Polarity is the main metric that indicates whether a piece of text is positive or negative. The polarity score (floating point value) lies on a range of [-1, 1] where positive is 1 and negative -1.

Subjectivity is a measure for opinion mining. A subjectivity score is a floating point value that lies on [0, 1], where 1 represents a strong opinion and 0 represents highly factual information.

For the purposes of this project, we use Textblob to calculate the polarity only as sentiment score.

As an aside, Textblob has a translator module which can be used to translate text to a desired language for further processing. However, I chose to use the Python 'translate' library instead for reasons discussed later below.

### VADER

NLTK VADER is similar to Textblob in which it calculates sentiment scores, but it provides a set of scores for different sentiments in a dictionary. These scores include negative, neutral, positive, and the cumulative lexicon-based compound score. For the purposes of this project, I use the compound score as a measure how positive or negative a piece of text is. 

The sentiment of a piece of text is determined as follows:
- if the compound score is less than or equal to -0.05, it is a negative sentiment
- if the compound score is between -0.05 and 0.05, it is a neutral sentiment,
- if the compound score is greater than or equal to 0.05, it is a positive sentiment

## Comparison between Textblob and VADER

![martymcfly](https://user-images.githubusercontent.com/3411100/86503845-112f5680-bd80-11ea-81c8-7c57d72114e0.png)

The sentiments returned by Textblob and VADER are not always the same. Textblob calculates polarity and subjectivity as sentiment analysis metrics. A possible explanation may be the available set of vocabularity for VADER to process. VADER works well with slangs and emojis, which are commonplace in social media platforms such as Twitter.

VADER is also more thorough with computing sentiments as it calculates individual sentiment scores (negative, neutral, and positive) as well as calculating the cumulative/compound score. This gives us a more complete understanding of the opinion expressed.

## Challenges

Building the data application was not without its limitations. During development the app would attempt to process tweets that simply cannot be processed. This returned a 400 error. I later created a try/except block to attempt to translate and if it couldn't, it would return a 'Cannot be processed' string in both Textblob and VADER sentiment columns:

![martymcfly](https://user-images.githubusercontent.com/3411100/86506083-04b7f780-bd9a-11ea-8a90-3bbbaaca3c00.png)

By entering the tweet URL in the browser, I can see that there are no textual contents.

Another issue I encountered during development was the use of Textblob's translate module. When I first started using it, the translation module worked before the Textblob or VADER could process the text. After 3 days later I tried the data app again it returned an error, with the message ```textblob.exceptions.NotTranslated: Translation API returned the input string unchanged.``` After much research, this turned out to be a known problem as addressed in this [StackOverflow forum](https://stackoverflow.com/questions/35420602/python-textblob-translation-api-error/35425227) - the Textblob translate module depends on the Google Translate API so  using it causes errors intermittently.

To find a stable library, I came across the Python 'translate' library and it has worked ever since. Whether or not it continues to work into the future, only time will tell.

## Some ideas for Improvement

- Build in custom time windows so that user can select the window of tweets to his or her liking. This helps the user to understand the context in a thread and the sentinent behind it
- Display the retweets related to the main tweets returned as they may be indicators of sentiment by popularity
- Implement WTForms in place of Flask request forms for validation. Besides form validation and error handling, WTForms offers features including CSRF and code injection, making it an all-around library to use. Error checks are done in the form entry, rather than passing the input to the application's server side and generating an HTTP error. This means that implementing WTForms would help the application to validate the input first before attempting a POST request.

## Conclusion

Although the libraries Textblob and VADER use algorithms for sentiment analysis, they may not always be accurate. This is why it helps to have a human eye to examine the raw tweet itself by looking at the text and going into the permalink. Perhaps there was a context behind the tweet being a reply to another.

But the app serves as a proxy to indicate whether a given piece of text on social media is negative, positive or neutral. It also serves as a good feasibility study into the minds of users and consumers and help organizations to respond accordingly. Beyond Twitter, sentiment analysis can be useful in a wide array of domains.
