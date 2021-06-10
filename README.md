## Non-answers: Measurement Approach

In "[Non-answers during Conference Calls](https://doi.org/10.1111/1475-679X.12371)", we classify a managerial response to a question as a non-answer using regular expressions to detect the presence of key phrases in the response.
Non-answers can take a number of forms.
Most non-answers contain explicit text indicating the speaker *refuses* to provide information, such as "we do not provide this disclosure" or "we do not disclose these numbers".
Other non-answers suggest the speaker was *unable* to provide the requested information, such as "I do not know" or "I can't give you any specifics".
A final, smaller category (*after-call*) involves an undertaking to provide the information after the conference call, such as "let's discuss it after the call" or "we could take that off-line".
The appendix to the paper provides examples and presents the set of regular expressions we use to identify non-answers.

## Gold standard

To develop our classification algorithm, we constructed a "gold standard" that was divided into training and test samples. 
To build our gold standard, we selected a random sample of 1,796 managerial responses.
Each response was examined by two workers on [CrowdFlower](https://www.crowdflower.com/), a crowdsourcing marketplace platform.
We asked each worker to identify any non-answers in the managerial response and to classify them into one of the three categories (refuses, unable, or after-call).
We had each worker record the shortest phrase from the response that justifies each non-answer classification they identified.
 
Once we collected data from the CrowdFlower platform, we asked skilled research assistants employed by the University of Chicago to examine all cases with inconsistent classifications by the CrowdFlower participants, as well as a random sample of additional cases. 
These research assistants resolved inconsistencies and finalized our "gold standard" corpus.
A key element of this "gold standard" corpus is an indicator variable *Non-answer* for each response, which takes a value of 1 if the response contains a non-answer, and 0 otherwise.

We then split our "gold standard" corpus into two sub-samples: a training sample comprising 1,296 responses and a test sample comprising 500 responses.
This gold sample is available here as [gold_standard.csv](https://github.com/iangow/non_answer_perf/blob/main/gold_standard.csv).

## Development of classification algorithm

We then manually developed a set of regular expressions based on manually identified non-answer phrases using the training sample until in-sample classification performance was deemed satisfactory.
Specifically, we sought *in-sample* classification accuracy over 90%. 
(Accuracy is defined as the proportion of responses correctly identified by the algorithm as containing non-answers or not.)
Once satisfactory performance was achieved, we fixed the regular expressions.

## Out-of-sample classification performance

After fixing the regular expressions, we applied our measure on the test (holdout) sample.
We then compared the *Non-answer* indicator implied by our regular expressions with the *Non-answer* indicator from our gold standard.

The performance statistics are reported in our paper and also in the Python notebook [here](https://github.com/iangow/non_answer_perf/blob/main/non_answer_perf.ipynb).
This notebook uses the `non_answer` function available in the `ling_features` Python package found [here](https://github.com/iangow/ling_features) and which can be installed using [Pip](https://pypi.org) (`pip install ling_features`).
