Virtually every brand, product, service, and even entertainment piece is online nowadays. Even so, consumers are talking about their experiences with a product online through review sites and social media. Whatever was written gets quickly spread online.

Companies are finding ways to monitor their brand online. Today there is a wealth of content to review for sentiment analysis. Therefore it is critical for companies to keep of reviews and any other social mentions.

I have built a sentiment analysis project to analyze tweets, as consumers tweet about brands everyday. 

The objective is simple: to determine if a given tweet about a company or brand is positive, negative, or neutral. I will elaborate below.

## Implementation

The project is built with Flask along with Textblob and VADER libraries. I chose Flask because it allows me to specify where and how to display the UI for the end user to perform sentiment analysis. Textblob and VADER are fairly popular and useful libraries and I like to use them to compare their sentiment results when processing a given piece of text.

### Textblob

Textblob computes polarity and subjectivity scores for a text. Polarity is the main metric that indicates whether a piece is positive or negative. The polarity score (floating point value) lies on a range of [-1, 1] where positive is 1 and negative -1.

Subjectivity is a measure for opinion mining. A subjectivity score is a floating point value that lies on [0, 1], where 1 represents an opinion and 0 represents factual information.

For the purposes of this project, we use Textblob to calculate the polarity only as sentiment score.

As an aside, Textblob has a translator module which can be used to translate text to a desired language for further processing.

### VADER

NLTK VADER is similar to Textblob in which it calculates sentiment scores, but it provides a set of scores for different sentiments in a dictionary. These scores include negative, neutral, positive, and the cumulative lexicon-based compound score. For the purposes of this project, I use the compound score as a measure how positive or negative a piece of text is. 

The sentiment of a piece of text is determined as follows:
- if the compound score is less than or equal to -0.05, it is a negative sentiment
- if the compound score is between -0.05 and 0.05, it is a neutral sentiment,
- if the compound score is greater than or equal to 0.05, it is a positive sentiment

## Comparison between Textblob and VADER

The sentiments returned by Textblob and VADER are not always the same. Textblob calculates polarity and subjectivity as sentiment analysis metrics. A possible explanation may be the available set of vocabularity for VADER to process. VADER works well with slangs and emojis, which are commonplace in social media platforms such as Twitter.

VADER is also more thorough with computing sentiments as it calculates individual sentiment scores (negative, neutral, and positive) as well as calculating the cumulative/compound score. This gives us a more accurate understanding of the opinion expressed.
