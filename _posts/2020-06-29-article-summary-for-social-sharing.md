Text summarization is a common branch of NLP where a piece of text is reduced to a compact corpus of text without compromising the key ideas of the original content. Given the explosion of written long form content, there is a growing need for summarization.

There are two types of summarization - extractive and abstractive. Extractive summarization scores sentences in an article and chooses the highest ranked phrases as concise text that underscores the meaning of the source text. However, it does so by mostly extracting sentences directly from the content to produce a summary, so the exact words are used. Much of the applied work on text summarization today is extractive summarization.

Abstractive summarization examines the source text and produces an original cohesive summary without necessarily relying on existing words from the source text, while still retaining the critical key ideas.

I have built a project that uses extractive summarization techniques to generate summary that a user can paste on their LinkedIn feeds. Sometimes I would scroll around on LinkedIn and found that the users would quote a phrase from a technical article along with its URL. I feel that this would be a fairly applicable use case where text summarization can reduce the time it takes to help users get their point across when engaging in social sharing.

## Implementation

The project uses the "PageRank" and TextRank algorithms to compute the summary of an article. Originally, PageRank works by deriving links between articles using a matrix to tabulate the backlinks used by each article. 

TextRank is derived from PageRank in that instead of ranking web pages, it is ranked by sentences. By converting sentences to word embeddings and computing their cosine similarity matrix scores, they would then be converted into a graph with sentences as vertices and edges as the scores. These scores represent the sentence rankings.

I implemented the "PageRank" version of the TextRank algorithm, as described in [this article](https://www.analyticsvidhya.com/blog/2018/11/introduction-text-summarization-textrank-python/). This breaks the steps required to produce a summary by selecting the top n sentences to present to the end user. The other implementation uses the ["summa summarizer"](https://github.com/summanlp/textrank) library that optimizes the similarity function and allows you to control and configure different parameters in summarizing an article. You could specify how many words, length as a proportion of the text, or even in a different language you want to use.

## Comparison between the two implementations

There are differences in how the sentences are ranked between the "PageRank" and "summa summarizer" implementations. A possible explanation may be that a variation of the cosine similarity was used as it was "optimized" as claimed. Depending on how it was implemented, the similarity function takes the sentence vectors and calculates the sentence ranking scores, so it is possible that a higher ranked sentence in one implementation may be ranked lower in the other.

Either way, the summaries are fairly plausible. If a user finds one compelling, he/she could take the summary with the link and share it on their LinkedIn (or other social platform) feed.
