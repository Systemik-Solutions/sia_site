# Burrows' Zeta

Burrows’ Zeta is a method for using the frequencies of mid-range words for a classification exercise. We take a Text Set of Class A samples and a Text Set of Class B samples. Which words which are used regularly in Class A and rarely in Class B, and vice-versa? Each sample in the analysis is then given two scores, based on how many of the words in the two lists occur in it.  A mystery sample which has many of the Class A words in its vocabulary and rather few of the Class B words can be assigned to Class A.

An example of how you might use Burrows’ Zeta is to solve an authorship problem. A new version of The Spanish Tragedy was published in 1602 with some added scenes. There is some evidence to connect them with Ben Jonson, but many commentators think they sound Shakespearean. We take a set of Shakespeare plays as Class A, and a set of Jonson plays as Class B, find the Class A words and the Class B words with the highest Zeta index scores, and see how many from each list are in the added scenes. How Shakespearean is the vocabulary of the added scenes, and alternatively how Jonsonian is it?

## Platform Process

1.	From the home page, select **Burrows’ Zeta**.
2.	Select **Text Sets** to **Add for Experiment**.
3.	Click **Edit** next to a Text Set to change its analysis criteria. Apply the following settings as required:
***
**Segmentation Options**

- **Segment by Tag** – This option is usually for Text Sets that include XML files that have been segmented as markup. ‘Tag’ refers to markup tags in XML that delineate a section of the text, e.g. ‘Act’. Type tags into this field to separate the Text Set into blocks divided by those tags.
- **Block Method** – Defines the method for creating blocks:
  - **BlockBigLast** – Assign any words left over from creating blocks to the last block, creating one large end block.
  - **BlockSmallLast** – Assign any words left over from creating blocks to their own block, creating one small end block.
  - **OverlappingBigLast** – Create blocks that overlap with each other. Any words that are left over from creating blocks are assigned to the last block, creating one large end block.
  - **OverlappingSmallLast** – Create blocks that overlap with each other. Any words that are left over from creating blocks are assigned to their own block, creating one small end block.
  - **Text** – Use the entirety of the Texts in the Text Set as single blocks.
- **Block Size** – Set the number of words to include in a block.
- **Advancement Size** – For overlapping blocks, denote the starting point of the next block, i.e. an advancement size of 100 would mean that each block starts 100 words after the start of the previous block.

**Word Selection Criteria** – Fill in the fields to define how words are selected in the Text Set.

**Test Segments**

- **Extract random test segments from this text set** – Check this box and set a **Test segment proportion** to extract random segments to test. The extracted segments play no part in the selection of the words for the analysis so are a genuine test of the method where it is confronted with freshly introduced samples. 
- **Zeta Index Calculation**
***
4.	Apply the following settings as required:
***
### General

**Zeta Parameters**

- **Output Size** – Set the number of Zeta Words that you want to include in the results.
- **Minimum Zeta score to output** – In the Zeta procedure each word is given a Zeta index score. This setting allows you to set a cut-off Zeta score, rather than a cut-off number of words to include.
- **Random seed for test segment extraction** – Once you have set a random seed, you will be able to return to the same random selection by entering the same seed next time. (Otherwise there is a separate random selection each time.)
- **Words** – Type words into this field to specify words to include or hide in the results, depending on the dropdown above.

**NGrams**

- **Number of NGrams** – Define the value of N for the NGram you want to analyse.

> **What is an NGram?** An NGram refers to a number of words in a sequence. An NGram of 1 can be called a 1Gram, and refers to a single word. A 5Gram is a sequence of 5 words, like “how is it with you” or “get thee to a nunnery”.

### Inclusions/Exclusions

Select XML tags to include or exclude in the results.

**Words to Exclude from Process** – Type words into this field to completely exclude from the Burrows’ Zeta process. By default, we exclude punctuation.

- **Include Homograph Forms** – Check this box to separate homograph forms of a word in the results (e.g. separate the noun and verb forms of “will”).
***
5.	Click **Show Results** to see the results on the right.

## Interpret the Results

Burrows’ Zeta results are split into a few tabs.

- **Zeta Experiment Summary** – This view lists the Zeta Words that have been analysed. Use the dropdown to see the **Hits**, **Frequencies**, and **Proportions** of each Zeta Word in each Text Set, as well as in any random test segments. **Hits** gives the numbers of segments that include one or more instances of the word, **Frequencies** gives the total of the instances of the word, and **Proportions** gives the total of the instances as a percentage of the size of the block.
- **Text Sets** – Each Text Set has its own result view, where you can see the frequencies of each Zeta Word in each segment/block of the Text Set. If you chose to extract random test segments from a Text Set, that will have its own tab as well.

Click **Copy to Clipboard** to copy the results so you can paste them into a document or spreadsheet.


## API Process 

`POST /zeta` 

Run Burrows' Zeta experiment on text sets. See Setting texts and text sets section about how to set text sets for the API. 

Each text set included in the experiment can have different options via `localOption`. There are also global settings from `globalOption` which apply to all text sets. 

**Request** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/request-zeta.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Request - Burrows' Zeta", 

    "description": "The request object to run Burrows' Zeta experiment.", 

    "type": "object", 

    "required": ["textSets"], 

    "properties": { 

        "textSets": { 

            "type": "array", 

            "items": { 

                "type": "object", 

                "description": "A text set with the local options.", 

                "required": ["textSet"], 

                "properties": { 

                    "textSet": { 

                        "description": "The text set.", 

                        "oneOf": [ 

                            { 

                                "type": "integer", 

                                "description": "The ID of the text set." 

                            }, 

                            {"$ref": "/api/v1/schemas/textset.schema.json"} 

                        ] 

                    }, 

                    "localOption": { 

                        "type": "object", 

                        "description": "The options which only apply to this text set.", 

                        "properties": { 

                            "segmentTags": { 

                                "type": "array", 

                                "description": "Segment the text by the XML tags provided", 

                                "items": { 

                                    "type": "string", 

                                    "description": "The name of the XML tag (without '<' and '>')" 

                                }, 

                                "default": [] 

                            }, 

                            "blockMethod": { 

                                "type": "integer", 

                                "description": "The method of segmenting the text into blocks.  (0):  Big Block Last (2):  Small Block Last (3): Segment by words of a given size ", 

                                "default": 2, 

                                "oneOf": [ 

                                    { 

                                        "const": 0, 

                                        "title": "By text", 

                                        "description": "Segment by the full content of texts. This is useful when segmenting text sets. But it can be also used on a single text resulting a single block." 

                                    }, 

                                    { 

                                        "const": 2, 

                                        "title": "Big block last", 

                                        "description": "Segment by words of a given size and the last block will have words of the given size plus the remaining words." 

                                    }, 

                                    { 

                                        "const": 3, 

                                        "title": "Small block last", 

                                        "description": "Segment by words of a given size and the last block will have the remaining words (which is less than the given size)." 

                                    }, 

                                    { 

                                        "const": 4, 

                                        "title": "Overlapping big block last", 

                                        "description": "Similar to the 'Big block last' method but with an advanced size defined when segmenting texts." 

                                    }, 

                                    { 

                                        "const": 5, 

                                        "title": "Overlapping small block last", 

                                        "description": "Similar to the 'Small block last' method but with an advanced size defined when segmenting texts." 

                                    } 

                                ] 

                            }, 

                            "blockSize": { 

                                "type": "integer", 

                                "description": "The number of words in each block when segementing the text. This is only in effect when the block method is 'Big block last' or 'Small block last'.", 

                                "exclusiveMinimum": 0, 

                                "default": 2500 

                            }, 

                            "advancedBlockSize": { 

                                "type": "integer", 

                                "description": "The number of words to advance when segmenting the next block. If the advanced size is smaller than the block size, each block will have an overlapping part with the neighbour blocks.", 

                                "exclusiveMinimum": 0, 

                                "default": 500 

                            }, 

                            "wordSelection": { 

                                "type": "object", 

                                "description": "The selection criteria of the Zeta words. Omit this property if no selection criteria is required.", 

                                "properties": { 

                                    "valueType": { 

                                        "type": "string", 

                                        "description": "The value type used for the word selection criteria.", 

                                        "default": "frequency", 

                                        "enum": ["frequency", "proportion"] 

                                    }, 

                                    "operator": { 

                                        "type": "string", 

                                        "description": "The operator used for the word selection criteria. The 'all' operator will include all words regardless of the criteria.", 

                                        "default": "all", 

                                        "enum": ["atleast", "nogreaterthan", "between", "all"] 

                                    }, 

                                    "min": { 

                                        "type": "number", 

                                        "description": "The minimal value of the criteria. When the value type is proportion, the values should be the decimal value of the proportion. Only applies when the operator is 'atleast' or 'between'.", 

                                        "default": 0, 

                                        "minimum": 0 

                                    }, 

                                    "max": { 

                                        "type": "number", 

                                        "description": "The maximal value of the criteria. When the value type is proportion, the values should be the decimal value of the proportion. Only applies when the operator is 'nogreaterthan' or 'between'.", 

                                        "default": 0, 

                                        "minimum": 0 

                                    }, 

                                    "scopeValueType": { 

                                        "type": "string", 

                                        "description": "The value type used for the scope constraint. Can either be the number of segments or the proportion among all segments.", 

                                        "default": "number", 

                                        "enum": ["number", "proportion"] 

                                    }, 

                                    "scopeOperator": { 

                                        "type": "string", 

                                        "description": "The operator used in the scope constraint. 'all' means applying to all segments.", 

                                        "default": "all", 

                                        "enum": ["all", "atleast", "atmost"] 

                                    }, 

                                    "scopeValue": { 

                                        "type": "number", 

                                        "description": "The number or proportion of segments used in the scope constraint. Only applies when the scope operator is 'atleast' or 'atmost'. Use the decimal value if the value is a proportion.", 

                                        "default": 0, 

                                        "minimum": 0 

                                    } 

                                } 

                            }, 

                            "extractTestSegments": { 

                                "type": "boolean", 

                                "description": "Whether to extract random test segments from this text set.", 

                                "default": false 

                            }, 

                            "testSegmentProportion": { 

                                "type": "number", 

                                "description": "The decimal value of the test segments proportion. Only applies when 'extractTestSegments' is set to 'true'.", 

                                "default": 0.1, 

                                "minimum": 0 

                            }, 

                            "isBaseTextSet": { 

                                "type": "boolean", 

                                "description": "Use this text set as the base text set.", 

                                "default": false 

                            }, 

                            "isCounterTextSet": { 

                                "type": "boolean", 

                                "description": "Use this text set as the counter text set.", 

                                "default": false 

                            } 

                        } 

                    } 

                } 

            } 

        }, 

        "globalOption": { 

            "type": "object", 

            "description": "Options applying to all text sets in the experiment.", 

            "properties": { 

                "outputSize": { 

                    "type": "integer", 

                    "description": "The number of Zeta words to output.", 

                    "default": 100, 

                    "exclusiveMinimum": 0 

                }, 

                "outputMinZetaScore": { 

                    "type": "integer", 

                    "description": "The minimal score of Zeta words for output.", 

                    "default": 0, 

                    "minimum": 0 

                }, 

                "testSegmentExtractionRandomSeed": { 

                    "type": "string", 

                    "description": "The seed used for selecting random test segments.", 

                    "default": "" 

                }, 

                "specialWords": { 

                    "type": "array", 

                    "description": "A list of special words to handle.", 

                    "items": { 

                        "type": "string" 

                    }, 

                    "default": [] 

                }, 

                "specialWordsOption": { 

                    "type": "integer", 

                    "description": "The way of handling special words.", 

                    "default": 0, 

                    "oneOf": [ 

                        { 

                            "const": 0, 

                            "description": "Include the special words, hide unspecified words" 

                        }, 

                        { 

                            "const": 2, 

                            "description": "Hide the special words, include unspecified words" 

                        } 

                    ] 

                }, 

                "numberOfNGrams": { 

                    "type": "integer", 

                    "description": "The number of n-grams used for frequencies.", 

                    "exclusiveMinimum": 0, 

                    "default": 1 

                }, 

                "excludeXMLTags": { 

                    "type": "array", 

                    "description": "List of XML tags to be excluded. Words within the specified tags will not be counted.", 

                    "default": [], 

                    "items": { 

                        "type": "string", 

                        "description": "The name of the XML tag (without '<' and '>')" 

                    } 

                }, 

                "ignoreXML": { 

                    "type": "boolean", 

                    "description": "Ignore all XML tags.", 

                    "default": false 

                }, 

                "excludeWords": { 

                    "type": "array", 

                    "description": "List of words to be excluded.", 

                    "default": [], 

                    "items": {"type": "string"} 

                }, 

                "includeHomograph": { 

                    "type": "boolean", 

                    "description": "Include the homograph forms.", 

                    "default": false 

                } 

            } 

        } 

    } 

} 
```

**Response** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/response-zeta.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Response - Burrows' Zeta", 

    "description": "The response object from the Burrows' Zeta feature.", 

    "type": "object", 

    "properties": { 

        "textSets": { 

            "type": "array", 

            "description": "The Burrows' Zeta results of all text sets.", 

            "items": { 

                "$ref": "#/$defs/zetaResult" 

            } 

        }, 

        "testSets": { 

            "type": "array", 

            "description": "The Burrows' Zeta results of test text sets with 'extractTestSegments' enabled in the request. Each test set will contain the zeta reulsts of randomly selected blocks.", 

            "items": { 

                "$ref": "#/$defs/zetaResult" 

            } 

        }, 

        "zetaScores": { 

            "type": "array", 

            "description": "Scores of zeta words. Only applies when the base and counter text sets are specified from the request.", 

            "items": { 

                "type": "object", 

                "description": "Scores of a zeta word", 

                "properties": { 

                    "word": { 

                        "type": "string", 

                        "description": "The zeta word." 

                    }, 

                    "training": { 

                        "type": "number", 

                        "description": "The training score of the zeta word." 

                    }, 

                    "test": { 

                        "type": "number", 

                        "description": "The test score of the zeta word." 

                    } 

                } 

            } 

        } 

    }, 

    "$defs": { 

        "zetaWord": { 

            "type": "object", 

            "description": "The zeta word with frequency data.", 

            "properties": { 

                "word": { 

                    "type": "string", 

                    "description": "The zeta word." 

                }, 

                "frequency": { 

                    "type": "integer", 

                    "description": "The frequency of the zeta word." 

                }, 

                "hits": { 

                    "type": "integer", 

                    "description": "The number of blocks where this zeta word appears." 

                }, 

                "proportion": { 

                    "type": "number", 

                    "description": "The decimal value of the frequency proportion." 

                } 

            } 

        }, 

        "zetaResult": { 

            "type": "object", 

            "description": "Burrows' Zeta result of a text set.", 

            "properties": { 

                "id": { 

                    "type": "integer", 

                    "description": "The ID of the text set. This is used for matching the test sets to their original text sets. If the text set is not stored in SIA, an arbitrary ID will be assigned." 

                }, 

                "name": { 

                    "type": "string", 

                    "description": "Name of the text set." 

                }, 

                "numOfBlocks": { 

                    "type": "integer", 

                    "description": "Number of segmented blocks in this text set." 

                }, 

                "zetaWords": { 

                    "type": "array", 

                    "description": "List of zeta words with frequency data in the text set.", 

                    "items": { 

                        "$ref": "#/$defs/zetaWord" 

                    } 

                }, 

                "blocks": { 

                    "type": "array", 

                    "items": { 

                        "type": "object", 

                        "description": "Burrows' Zeta result of a text block from the text set.", 

                        "properties": { 

                            "name": { 

                                "type": "string", 

                                "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                            }, 

                            "wordCount": { 

                                "type": "integer", 

                                "description": "The number of words in this block." 

                            }, 

                            "wordProportion": { 

                                "type": "number", 

                                "description": "The proprtion of zeta words to all words from this block." 

                            }, 

                            "uniqueWordCount": { 

                                "type": "integer", 

                                "description": "The number of unique words in this block." 

                            }, 

                            "uniqueWordProportion": { 

                                "type": "number", 

                                "description": "The proprtion of zeta words to unique words from this block." 

                            }, 

                            "zetaWords": { 

                                "type": "array", 

                                "description": "List of zeta words with frequency data in this block.", 

                                "items": { 

                                    "$ref": "#/$defs/zetaWord" 

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
