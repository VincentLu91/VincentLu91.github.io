Text summarization is a common branch of NLP where a piece of text is reduced to compact corpuses of text without compromising the key ideas of the source content. Given the explosion of written long form content, there is a growing need for summarization.

There are two types of summarization - extractive and abstractive. Extractive summarization scores sentences in an article and chooses the highest ranked phrases as concise pieces of text that underscore the meaning of the source text. However, it does so by mostly extracting sentences directly from the content to produce a summary, so the exact words are used. Much of the applied work on text summarization today is done on extractive summarization.

Abstractive summarization examines the source text and produces an original cohesive summary without necessarily relying on existing words from the source text, while still retaining the critical key ideas.

I have built a data app that uses extractive summarization techniques to generate summary that a user can paste on their LinkedIn feeds. Sometimes I would scroll around on LinkedIn and found that the users would quote a phrase from a technical article along with its URL. I feel that this would be a fairly applicable use case where text summarization can reduce the time it takes to help users get their point across when engaging in social sharing.

Data app is deployed and can be accessed: https://technical-summary-proj.herokuapp.com.

## Implementation

The data app uses the TextRank algorithm to compute the summary of an article. TextRank is derived from PageRank in that instead of ranking web pages, it is ranked by sentences. By converting sentences to word embeddings and computing their cosine similarity matrix scores, they would then be converted into a graph with sentences as vertices and edges as the scores. These scores represent the sentence rankings.

The library used for this is the ["summa summarizer"](https://github.com/summanlp/textrank). It optimizes the similarity function and allows for control and configure different parameters in summarizing an article. This includes number of words, length as a proportion of the text, or even in a different language (for source text only). The app is written in Flask.

![martymcfly](https://user-images.githubusercontent.com/3411100/86506899-c8889500-bda1-11ea-85f0-21717e8531c8.png)

## Challenges

Initially, I had planned for the text summarizer to produce a fix number of summaries - 10. However, this results in an 'list out of range' error when the number of summaries produced is fewer than 10. As such, I removed the restriction and allowed the summarizer to generate as many summaries as needed - more or less than 10.

I had also originally considered making this summarizer to be only for posting summary posts on Twitter, however this proven to be unfeasible as the length of the summary can vary. Currently the NLP library used allows for the length of summary expressed in terms of number of words or a fractional proportion of the source content. So, I widened the use case to allow for Facebook and LinkedIn. A user can still certainly post a summary post on Twitter, but he/she would need to be sure that the number of characters is not greater than 140.

## Some thoughts for Improvement

- allow the user to choose between which of the 3 platforms (Facebook, LinkedIn, Twitter) to post so the the data app can accordingly limit the number of characters in returned summaries. Users can then decide which summary to use for posting
- experiment with summarization in different languages. One feasible workaround would be to take the list of summaries generated from source text and apply a translation algorithm to each of them. This could be from English to French, German to Portuguese, Chinese to English etc.

## Conclusion

While there are still improvements to be made in the field, some implementations have shown potential.

My experience with TextRank is that it has been been reliable in generating phrases through extractive summarization. This is mostly applicable to social media: occasionally a user would directly quote a phrase from an article that he/she feels would resonate with the audience. 

However, this does not mean that abstractive summarization should be excluded from this domain. Until there is significant progress made, it might be useful to generate 'tl;dr' type of posts for a user's followers to read.

With time, text summarization can be applied to almost any domain imaginable.
