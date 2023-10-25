# PragmatiCQA

This repository contains the dataset and code for the paper "[PragmatiCQA: A Dataset for Pragmatic Question Answering in Conversations](https://aclanthology.org/2023.findings-acl.385.pdf)" published at Findings of ACL 2023.

PragmatiCQA is a conversational question answering dataset that features answers annotated for not only the literal interpretation of the question, but also pragmatic inferences that humans perform naturally in conversations that can greatly improve the efficiency of communication. See the figure below for an example.

![An example of the kinds of pragmatic behavior captured by the conversational questions in PragmatiCQA. The question is "Is there water on Mars?" If one were to interpret this question literally, the direct answer would be "Yes, there is water on Mars." However, by pragmatically inferring about potential follow-up questions like "Where? In what form?" and factoring in relevant knowledge like "Water has been found in 23 places in our Solar System. Turns out it isn't so parched.", a helpful human assistant might be able to provide the following pragmatic answer: "Yes, but only in the form of ice caps near its poles. In fact, Mars is just one of 23 places where we have found water in the Solar System!](https://github.com/qipeng/PragmatiCQA/blob/main/images/example.png?raw=true "An example of the kinds of pragmatic behavior captured by the conversational questions in PragmatiCQA")

## Download the Data

The conversational question answering dataset (train/dev/test split) can be found in the `data/` directory of this repository.

Each of the data files contains one conversation per line in a JSON format that looks like the following:

```
{
    "topic": "[This is the page title in Fandom that this conversation started with, typically the main entity of that community]",
    "genre": "[This is Fandom's classification of genre for the community, usually TV, Music, Lifestyle, etc.]",
    "community": "[Community name in Fandom]"
    "qas": [ // This is the entire conversation, one question-answer pair per item
        {
            "q": "[question from the student]",
            "a_meta": {
                "literal_obj": [ // These are the spans from the corpus that answers the question when it is interpreted literally
                    {
                        "text": "[span from the corpus]",
                        "startKey": "[element UUID from the corpus]",
                        "endKey": "[element UUID from the corpus]"
                    }
                ],
                "pragmatic_obj": [ // These are the spans from the corpus that go beyond the literal answer to address potential follow-up questions or provide relevant information from the corpus to keep the conversation engaging
                    {
                        "text": "[span from the corpus]",
                        "startKey": "[element UUID from the corpus]",
                        "endKey": "[element UUID from the corpus]"
                    }
                ]
            },
            "a": "[final answer combining information from both literal and pragmatic spans, paraphrased into a consistent, natural sounding answer]",
            "human_eval": [ // Ratings from the Student of the entire Teacher response.
            ]
        },
        ...
    ]
}
```

The corpus can be downloaded [here](https://drive.google.com/file/d/17vbeArdufh8rfhkg2I4Mwm0C9Og5klFR/view?usp=drive_link), where the HTML files of relevant communities used in PragmatiCQA are scraped and pre-processed with UUIDs added to each DOM element. The `"startKey"` and `"endKey"` UUIDs in the data files refer to these DOM UUIDs to identify where the spans were originally selected from to avoid potential issues with weak supervision.

### Human Evaluation of the Answers

As the conversation takes place, we ask the Student crowd worker to rate the previous response from the Teacher crowd worker while they work on providing a response to the current question.

Each response is presented with five questions, each rated at a 5-level Likert scale:

1. Does the previous response from the Teacher sound like it was copied and pasted from an article, or sound natural as humans would say in a conversation? (5=Very Copy-Paste, 1=Very Conversational)

2. Does the literal answer (spans provided) answer your question without providing excess information, if one were to interpret the question literally? (5=Completely Disagree, 1=Completely Agree)

3. Do the snippet(s) in (pragmatic spans provided) go beyond the literal answer (spans provided) to anticipate and answer would-be questions? (5=Completely Disagree, 1=Completely Agree)

4. Do the snippet(s) in (pragmatic spans provided) go beyond the literal answer (spans provided) to provide helpful leads that help you ask further questions on this topic? (5=Completely Disagree, 1=Completely Agree)

5. Does the final response faithfully represent information in both the literal spans and the pragmatic spans? (5=Completely Disagree, 1=Completely Agree)

## License

The PragmatiCQA dataset is licensed under the [Creative Commons Attribution-Share Alike (CC BY-SA) License](https://creativecommons.org/licenses/by-sa/4.0/).

The code in this repository is licensed under the [Apache v2 License](https://www.apache.org/licenses/LICENSE-2.0) with copyright owned by the original authors of PragmatiCQA.