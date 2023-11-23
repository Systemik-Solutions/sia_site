# Divergence

Jensen-Shannon Divergence (JSD, or simply Divergence) is a measure of the similarity or difference between two Text Sets. It is based on proportional frequencies of words. It provides a single measure of difference between samples relying on multiple component word frequencies. A typical application is to compare all the words in a collection to the overall average of that collection. Which words are typical, and which words are aberrant?

By looking at the individual word-variables which contribute most to the Divergence score you can quickly determine whether a handful of words have largely created the divergence, or not, and look at what these words are – typically a very common word with a very high or very low proportional frequency, or a usually rare word used intensively in one of the Text Sets.

An example of a use for Divergence would be to find which is the most ‘normal’ segment in a set of Shakespeare plays – the segment that is closest to the overall average word use – and which segment is the most ‘aberrant’.

## Platform Process

1.	From the home page, select **Divergence**.
2.	Select a **Reference Text Set** as the base Text Set and a **Comparison Text Set** to compare with it.
3.	Apply the following settings as required:
***
### General

**XML Options** – This option is for Text Sets that include XML files that have been segmented as markup. Select XML tags to include or exclude in the results.

**Segmentation Options**

- **Segment by Character** – This option is usually for Text Sets that include plays, or XML files that have been segmented as markup. Check this box to separate the Text Sets into blocks divided by speaking character.
- **Block Size** – Set the number of words to include in a block.

**Count Options**

- **Number of NGrams** – Define the value of N for the NGram you want to analyse.

> **What is an NGram?** An NGram refers to a number of words in a sequence. An NGram of 1 can be called a 1Gram, and refers to a single word. A 5Gram is a sequence of 5 words, like “how is it with you” or “get thee to a nunnery”.

- **Count Graphemes** – We strongly recommend that you uncheck this box. If you leave it checked, Divergence will use graphemes/letters as a Gram (i.e. ‘the’ will become a 3Gram).

**Word Options** – Type words into this field to completely exclude from the Divergence process. By default, we exclude punctuation.

- **Include Homograph Forms** – Check this box to separate homograph forms of a word in the results (e.g. separate the noun and verb forms of “will”).

**Divergence Options**

- **Log Base** – The Jensen-Shannon Divergence formula includes a log function. You can choose to use logs to the base 2, base 10 or base ‘e’ (i.e., a natural logarithm).
- **Per Word Calculation** – Check this box to see which words contributed most to the Divergence score in the results.
- **Log N/M Calculation**
- **Log N/M Mode**

**Output Options**

- **Show Metadata** – Check this box to show metadata from the Text Sets in the results, e.g., the Titles of Texts.

### Filter

**Words** – Type words into this field to specify words to include or hide in the results, depending on the dropdown above.

**Use most frequent words** – Check this box to only use the most frequent words to determine Divergence. Specify the number of words in the Size field.
***
5.	Click **Show Results** to see the results on the right.

## Interpret the Results

Divergence results are split into many tabs.

- **Divergence Table** and **Divergence Column** – These views show the Divergence scores for segments/blocks of the Comparison Text Set. A higher score means the segment diverges more from the Reference Text Set.
- **Reference Entropy Table**, **Reference Entropy Column**, **Reference Weighted Entropy Table** and **Reference Weighted Entropy Column** – These views assign each segment of the Reference Text Set with an Entropy score, which indicates how repetitive the word use is within the segment. A higher score means the word use is less repetitive. The **Reference Entropy Table** includes an Average score.
- **Comparison Entropy Table**, **Comparison Entropy Column**, **Comparison Weighted Entropy Table** and **Comparison Weighted Entropy Column** – These views assign each segment of the Comparison Text Set with an Entropy score, which indicates how repetitive the word use is within the segment. A higher score means the word use is less repetitive. The **Comparison Entropy Table** includes an Average score.

Click **Copy to Clipboard** to copy the results so you can paste them into a document or spreadsheet.

## API Process 

`POST /divergence` 

Run divergence based on a reference text set and a comparison text set. See Setting texts and text sets section about how to set the `referenceTextSet` and `comparisonTextSet` for the API. 

The divergence calculations are based on the word frequencies. Therefore, the options for running the word frequencies can be set by the property `wordFrequenciesOption`. 

**Request** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/request-divergence.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Request - concordance", 

    "description": "The request object to run divergence.", 

    "type": "object", 

    "required": ["referenceTextSet", "comparisonTextSet"], 

    "properties": { 

        "referenceTextSet": { 

            "description": "The reference text set used for divergence.", 

            "oneOf": [ 

                { 

                    "type": "integer", 

                    "description": "The ID of the text set." 

                }, 

                {"$ref": "/api/v1/schemas/textset.schema.json"} 

            ] 

        }, 

        "comparisonTextSet": { 

            "description": "The comparison text set used for divergence.", 

            "oneOf": [ 

                { 

                    "type": "integer", 

                    "description": "The ID of the text set." 

                }, 

                {"$ref": "/api/v1/schemas/textset.schema.json"} 

            ] 

        }, 

        "wordFrequenciesOption": {"$ref": "/api/v1/schemas/wfoption.schema.json"}, 

        "divergenceOption": { 

            "logBase": { 

                "type": "string", 

                "description": "The base of logarithm used for the divergence score calculation.", 

                "default": "e", 

                "enum": ["e", "10", "2"] 

            }, 

            "perWordCalculation": { 

                "type": "boolean", 

                "description": "Output the divergence score for each individual word within the text block.", 

                "default": false 

            }, 

            "logNMCalculation": { 

                "type": "boolean", 

                "description": "Use the logN/M calculation for divergence, which is affected by the weights of words based on the logN/M mode.", 

                "default": false 

            }, 

            "logNMMode": { 

                "type": "integer", 

                "description": "The mode of the logN/M calculation. Only applies when logN/M calculation is enabled.", 

                "default": 0, 

                "oneOf": [ 

                    { 

                        "const": 0, 

                        "title": "Comparison", 

                        "description": "Use the word weights from the comparison text set." 

                    }, 

                    { 

                        "const": 1, 

                        "title": "Reference", 

                        "description": "Use the word weights from the reference text set." 

                    }, 

                    { 

                        "const": 2, 

                        "title": "Union", 

                        "description": "Use the word weights from the union of reference and comparison text sets." 

                    } 

                ] 

            }, 

            "useMostFrequentWords": { 

                "type": "boolean", 

                "description": "Only use the most frequent words for divergence.", 

                "default": false 

            }, 

            "mostFrequentWordSize": { 

                "type": "integer", 

                "description": "The size of most frequent words used for divergence. Only applies when useing the most frequent words for divergence.", 

                "default": 100, 

                "exclusiveMinimum": 0 

            } 

        } 

    } 

} 
```

**Response** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/response-divergence.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Response - divergence", 

    "description": "The response object from the divergence feature.", 

    "type": "object", 

    "properties": { 

        "divergence": { 

            "type": "array", 

            "description": "The divergence results of texts from the comparison text sets.", 

            "items": { 

                "type": "object", 

                "description": "The divergence results of a single text from the comparison text sets.", 

                "properties": { 

                    "text": { 

                        "type": "string", 

                        "description": "The title of the text." 

                    }, 

                    "blocks": { 

                        "type": "array", 

                        "items": { 

                            "type": "object", 

                            "description": "The divergence results of a block from the text.", 

                            "properties": { 

                                "sequenceNo": { 

                                    "type": "integer", 

                                    "description": "The sequence number of the block within the text." 

                                }, 

                                "name": { 

                                    "type": "string", 

                                    "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                                }, 

                                "divergence": { 

                                    "type": "number", 

                                    "description": "Divergence score of the block." 

                                }, 

                                "wordDivergence": { 

                                    "type": "object", 

                                    "description": "The divergence score of each word from the block, if 'perWordCalculation' is set to 'true' in the request.", 

                                    "additionalProperties": {"type": "number"} 

                                } 

                            } 

                        } 

                    } 

                } 

            } 

        }, 

        "referenceShannonEntropy": { 

            "type": "array", 

            "description": "The Shannon entropy of texts from the reference text sets.", 

            "items": { 

                "type": "object", 

                "description": "The Shannon entropy of a single text from the reference text sets.", 

                "properties": { 

                    "text": { 

                        "type": "string", 

                        "description": "The title of the text." 

                    }, 

                    "blocks": { 

                        "type": "array", 

                        "items": { 

                            "type": "object", 

                            "description": "The Shannon entropy of a block from the text.", 

                            "properties": { 

                                "sequenceNo": { 

                                    "type": "integer", 

                                    "description": "The sequence number of the block within the text." 

                                }, 

                                "name": { 

                                    "type": "string", 

                                    "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                                }, 

                                "entropy": { 

                                    "type": "number", 

                                    "description": "Entropy of the block." 

                                } 

                            } 

                        } 

                    } 

                } 

            } 

        }, 

        "comparisonShannonEntropy": { 

            "type": "array", 

            "description": "The Shannon entropy of texts from the comparison text sets.", 

            "items": { 

                "type": "object", 

                "description": "The Shannon entropy results of a single text from the comparison text sets.", 

                "properties": { 

                    "text": { 

                        "type": "string", 

                        "description": "The title of the text." 

                    }, 

                    "blocks": { 

                        "type": "array", 

                        "items": { 

                            "type": "object", 

                            "description": "The Shannon entropy of a block from the text.", 

                            "properties": { 

                                "sequenceNo": { 

                                    "type": "integer", 

                                    "description": "The sequence number of the block within the text." 

                                }, 

                                "name": { 

                                    "type": "string", 

                                    "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                                }, 

                                "entropy": { 

                                    "type": "number", 

                                    "description": "Entropy of the block." 

                                } 

                            } 

                        } 

                    } 

                } 

            } 

        }, 

        "referenceWeightedEntropy": { 

            "type": "array", 

            "description": "The weighted entropy of texts from the reference text sets.", 

            "items": { 

                "type": "object", 

                "description": "The weighted entropy of a single text from the reference text sets.", 

                "properties": { 

                    "text": { 

                        "type": "string", 

                        "description": "The title of the text." 

                    }, 

                    "blocks": { 

                        "type": "array", 

                        "items": { 

                            "type": "object", 

                            "description": "The weighted entropy of a block from the text.", 

                            "properties": { 

                                "sequenceNo": { 

                                    "type": "integer", 

                                    "description": "The sequence number of the block within the text." 

                                }, 

                                "name": { 

                                    "type": "string", 

                                    "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                                }, 

                                "entropy": { 

                                    "type": "number", 

                                    "description": "Entropy of the block." 

                                } 

                            } 

                        } 

                    } 

                } 

            } 

        }, 

        "comparisonWeightedEntropy": { 

            "type": "array", 

            "description": "The weighted entropy of texts from the comparison text sets.", 

            "items": { 

                "type": "object", 

                "description": "The weighted entropy results of a single text from the comparison text sets.", 

                "properties": { 

                    "text": { 

                        "type": "string", 

                        "description": "The title of the text." 

                    }, 

                    "blocks": { 

                        "type": "array", 

                        "items": { 

                            "type": "object", 

                            "description": "The weighted entropy of a block from the text.", 

                            "properties": { 

                                "sequenceNo": { 

                                    "type": "integer", 

                                    "description": "The sequence number of the block within the text." 

                                }, 

                                "name": { 

                                    "type": "string", 

                                    "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                                }, 

                                "entropy": { 

                                    "type": "number", 

                                    "description": "Entropy of the block." 

                                } 

                            } 

                        } 

                    } 

                } 

            } 

        } 

    } 

}
```
