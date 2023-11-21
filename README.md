# WikiQA
A Wikipedia Q&amp;A Test Corpus. This test corpus contains a [Vectra Index](https://github.com/Stevenic/vectra) of the first 50 documents in the [WikiQA Test Corpus](https://www.kaggle.com/datasets/saurabhshahane/wikiqa-corpus) from Microsoft Research.

## Usage
Clone the repo and unzip the `vectra-index.zip` file which will add a folder called `wikiqa` to the project. This is the local Vector DB for the first 50 documents in [wikiqa.xls](wikiqa.xls).

Next install the Vectra CLI:

```bash
npm install -g vectra
```

Next copy `vectra.keys.example` to a file called `vectra.keys` and replace the "<OPENAI_KEY>" placeholder with your OpenAI key.

You can now use the Vectra CLI to query the index:

```bash
vectra query wikiqa "how did apollo creed die" -k vectra.keys
```

The default is to return query results using Vectras Document Sections algorithm so if you'd like to see the raw chunks being predicted for evaluation purposes you can choose to see the results in chunk order:

```bash
vectra query wikiqa "how did apollo creed die" -k vectra.keys -f chunks
```

You can also ask for the stats from a query which will give you a list of documents predicted, how many chunks per document were predicted, and what was the average score of those chunks:

```bash
vectra query wikiqa "how did apollo creed die" -k vectra.keys -f stats
```

## Adding Additional Documents
You can add additional documents to the corpus by creating a text file called `wikiqa.additional.links` with the wikipedia links you'd like to add. Look at the `wikiqa.links` file for example formatting but it's just one URL per line.

To add the documents to the index run:

```bash
vectra add wikiqa -k vectra.keys -l wikiqa.additional.links
```

Vectra will crawl the docs into the index.  It converts each HTML page to markdown and then splits the markdown file into chunks and generates an embedding for each chunk.  The cap for a single vectra index is around 2000 documents which results in an index that's several gb in size.