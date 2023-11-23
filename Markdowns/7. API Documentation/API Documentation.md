# API Documentation

## API base path

`/api/v1/`

## API endpoints

### Texts

#### View all

`GET /texts`

List all texts owned by the authenticated user. 

**URL Parameters**

`showcontent`: setting to `1` will include the content of texts in the response. By default, it is set to `0` to omit the content of texts. 

**Response** 

An array of text objects. 

##### View

`GET /texts/{id}`

View the information of a text owned by the authenticated user. `{id}` is the ID of text. 

**Response**

The text object of the given ID. 

##### Create 

`POST /texts` 

Create a new text. 

**Request** 

The request body should be a text object for the new text. The text object should omit all `readOnly` properties, and they will be ignored even if they exist in the request body. 

**Response** 

The text object of the newly created text. 

#### Update 

`PUT /texts/{id}`

Update a text owned by the authenticated user. `{id}` is the ID of text. 

**Request** 

A text object with only the updated properties. Properties not specified in the request body will remain untouched. 

**Response** 

The full text object of the updated text. 

#### Delete 

`DELETE /texts/{id}` 

Delete a text owned by the authenticated user. `{id}` is the ID of text. 

**Response** 

The full text object of the deleted text. 

### Text Sets

#### View all 

`GET /text-sets` 

List all text sets owned by the authenticated user. 

**URL Parameters** 

`showcontent`: setting to `1` will include the content of texts in the response. By default, it is set to `0` to omit the content of texts. 

**Response** 

An array of text set objects. 

#### View 

`GET /text-sets/{id}` 

View the information of a text set owned by the authenticated user. `{id}` is the ID of text set. 

**URL Parameters** 

`showcontent`: setting to `1` will include the content of texts in the response. By default, it is set to `0` to omit the content of texts. 

**Response** 

The text set object of the given ID. 

#### Create 

`POST /text-sets` 

Create a new text set. 

**Request** 

A text set object for the new text set. The object should omit all `readOnly` properties, and they will be ignored even if they exist in the request body. 

The `texts` property of the text set object can contain the list of text objects to be included in the text set. If the `id` is valid and specified in a text object, it will associate that existing text to the text set. Otherwise, it will try to create the text first and then associate the text to the text set. 

**Response** 

The text set object of the newly created text set. For each text object in the text set, it will omit the content of the text. To get the content of a text, use the view text API. 

#### Update 

`PUT /text-sets/{id}` 

Update a text set owned by the authenticated user. `{id}` is the ID of text set. 

**Request** 

A text object with only the updated properties. Properties not specified in the request body will remain untouched. 

Similar to the create API, `id` of text objects can be set for existing texts. Or it will create new texts for the text set. Note that by setting up the texts property, it will overwrite the entire collection of texts for the text set. 

**Response** 

The text set object of the updated text set. For each text object in the text set, it will omit the content of the text. To get the content of a text, use the view text API. 

#### Delete 

`DELETE /text-sets/{id}` 

Delete a text set owned by the authenticated user. `{id}` is the ID of text set. Note that this will only unlink the texts from the deleted text set. To also delete the texts in the text set, use the delete text API. 

**Response** 

The text set object of the deleted text set. For each text object in the text set, it will omit the content of the text. 

### Features

#### Concordance

`POST /concordance` 

Running concordance on a single text. See Setting texts and text sets section about how to set text property. 

**Request** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/request-concordance.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Request - concordance", 

    "description": "The request object to run concordance.", 

    "type": "object", 

    "required": ["text", "option"], 

    "properties": { 

        "text": { 

            "description": "Specifies the text for the concordance.", 

            "oneOf": [ 

                { 

                    "type": "integer", 

                    "description": "The ID of the text." 

                }, 

                {"$ref": "/api/v1/schemas/text.schema.json"} 

            ] 

        }, 

        "option": { 

            "type": "object", 

            "description": "Concordance settings", 

            "required": ["words"], 

            "properties": { 

                "words": { 

                    "type": "array", 

                    "description": "The list of words to be searched.", 

                    "items": { 

                        "type": "string" 

                    } 

                }, 

                "numOfWordsBefore": { 

                    "type": "integer", 

                    "description": "The number of words to return in the pre-context.", 

                    "default": 5, 

                    "exclusiveMinimum": 0 

                }, 

                "numOfWordsAfter": { 

                    "type": "integer", 

                    "description": "The number of words to return in the post-context.", 

                    "default": 5, 

                    "exclusiveMinimum": 0 

                } 

            } 

        } 

    } 

}
```

**Response**

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/response-concordance.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Response - concordance", 

    "description": "The response object from the concordance feature.", 

    "type": "object", 

    "properties": { 

        "words": { 

            "type": "array", 

            "description": "The list of words with their contexts.", 

            "items": { 

                "type": "object", 

                "properties": { 

                    "word": { 

                        "type": "string", 

                        "description": "The searched word." 

                    }, 

                    "preContext": { 

                        "type": "array", 

                        "description": "List of words before the searched word.", 

                        "items": { 

                            "type": "string" 

                        } 

                    }, 

                    "postContext": { 

                        "type": "array", 

                        "description": "List of words after the searched word.", 

                        "items": { 

                            "type": "string" 

                        } 

                    } 

                } 

            } 

        } 

    } 

}
```

#### Word Frequencies

`POST /word-frequencies` 

Run word frequencies on texts or text sets. The word frequencies can be run on either texts or text sets. See Setting texts and text sets section about how to set the `text` or `textSet` property for the API. However, both `text` and `textSet` cannot be set at the same time in a API call. 

The `options` of word frequencies can be set up by the option property with a word frequency option object. 

**Request** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/request-wf.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Request - word frequencies", 

    "description": "The request object to run word frequencies.", 

    "type": "object", 

    "oneOf": [ 

        { 

            "required": ["text"] 

        }, 

        { 

            "required": ["textSet"] 

        } 

    ], 

    "properties": { 

        "text": { 

            "description": "Set the text for word frequencies.", 

            "oneOf": [ 

                { 

                    "type": "integer", 

                    "description": "The ID of the text." 

                }, 

                {"$ref": "/api/v1/schemas/text.schema.json"} 

            ] 

        }, 

        "textSet": { 

            "description": "Specify a text set instead of a text for word frequencies.", 

            "oneOf": [ 

                { 

                    "type": "integer", 

                    "description": "The ID of the text set." 

                }, 

                {"$ref": "/api/v1/schemas/textset.schema.json"} 

            ] 

        }, 

        "option": {"$ref": "/api/v1/schemas/wfoption.schema.json"} 

    } 

} 
```

**Response** 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/response-wf.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Response - word frequencies", 

    "description": "The response object from the word frequencies feature.", 

    "type": "object", 

    "properties": { 

        "blocks": { 

            "type": "array", 

            "description": "Word frequency results of segmented texts.", 

            "items": { 

                "type": "object", 

                "description": "The data of a block", 

                "properties": { 

                    "name": { 

                        "type": "string", 

                        "description": "The name of the text block in the format of 'Text title (Sequence Number)'." 

                    }, 

                    "size": { 

                        "type": "integer", 

                        "description": "The number of words in this block." 

                    }, 

                    "uniqueWordCount": { 

                        "type": "integer", 

                        "description": "The number of unique words in this block." 

                    }, 

                    "shannonEntropy": { 

                        "type": "number", 

                        "description": "The shannon entropy of the word frequencies in the current block." 

                    }, 

                    "start": { 

                        "type": "array", 

                        "description": "The first four words of the block.", 

                        "items": { 

                            "type": "string" 

                        } 

                    }, 

                    "end": { 

                        "type": "array", 

                        "description": "The last three words of the block.", 

                        "items": { 

                            "type": "string" 

                        } 

                    }, 

                    "frequencies": { 

                        "type": "array", 

                        "description": "Word frequencies of the current block.", 

                        "items": { 

                            "type": "object", 

                            "description": "The frequency of a single word in the current block.", 

                            "properties": { 

                                "word": { 

                                    "type": "string", 

                                    "description": "The word." 

                                }, 

                                "value": { 

                                    "type": "integer", 

                                    "description": "The frequency of the word." 

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

#### Divergence

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

#### Burrows' Zeta

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

## Setting texts and text sets

Texts can be specified via one of the following methods when calling the feature API such as word frequencies API: 

- using an existing text from SIA by specifying the `id` of the text.
- using a constructed text object. `readOnly` properties of the text object are not required in this case. The process will use the content and metadata from the text object. However, it will not save the text data into SIA. 

Text sets can be specified in similar methods: 

- using an existing text set from SIA by providing the `id` of the text set.
- using a constructed text set object.

## Authentication

To use the API, users need to go to their account page in SIA to create the API keys. SIA will then generate the API key for the user. The custom HTTP header `X-API-KEY` must be set up with the API key string when sending requests to API. The API have the access of all assets of the authenticated user. 

## Error responses

The following HTTP response code can be produced by the API when errors occur. 


| Code  | Description |
| ------------- | ------------- |
| 400 Bad Request  | Incorrect or incomplete request data.  |
| 401 Unauthorized  | Errors occurred during the API authentication process.  |
| 403 Forbidden  | The user doesn't have the permission to access the requested resources such as texts and text sets.   |
| 404 Not Found  | The requested resources such as texts and text sets are not found.  |
| 500 Internal Server Error  | Errors occurred during the API run time.  |

The response body may contain an object which have the list of detailed error messages when errors occur. 

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/error.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Response - error", 

    "description": "The error response object.", 

    "type": "object", 

    "properties": { 

        "errors": { 

            "type": "array", 

            "items": { 

                "type": "string", 

                "description": "The error message." 

            } 

        } 

    } 

}
```

## JSON object types

This section documents the JSON schema of objects commonly used in SIA API requests and responses. 

### Texts

A text object represents a text record from SIA which contains the metadata and content of the text. Texts are the base unit of feature processing in SIA. 

#### JSON schema

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/text.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Text", 

    "description": "The text object from Intelligent Archive", 

    "type": "object", 

    "required": ["title", "content", "format", "encoding"], 

    "properties": { 

        "id": { 

            "type": "integer", 

            "description": "The ID of the text", 

            "readOnly": true 

        }, 

        "title": { 

            "type": "string", 

            "description": "The title of the text" 

        }, 

        "author": { 

            "type": "string", 

            "description": "The author of the text" 

        }, 

        "content": { 

            "type": "string", 

            "description": "The content of the text" 

        }, 

        "format": { 

            "type": "string", 

            "description": "The format of the text. Currently only 'TEI' and 'Plain Text' are supported.", 

            "enum": ["TEI", "Plain Text"] 

        }, 

        "encoding": { 

            "type": "string", 

            "description": "The encoding of the text content. Currently only 'UTF-8' is supported.", 

            "enum": ["UTF-8"] 

        }, 

        "metadata": { 

            "type": "object", 

            "description": "The key:value pairs of the custom metadata.", 

            "additionalProperties": {"type": "string"} 

        }, 

        "created_at": { 

            "type": "string", 

            "description": "The time when the text record is created in the system.", 

            "readOnly": true 

        }, 

        "updated_at": { 

            "type": "string", 

            "description": "The last time when the text record is updated in the system.", 

            "readOnly": true 

        } 

    } 

}
```

### Text Sets

A text set is a collection of text in SIA. Features from SIA can process and generate output on multiple texts through text sets. 

#### JSON schema

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/textset.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Text set", 

    "description": "The text set object from Intelligent Archive. A text set is a collection of one or multiple text objects.", 

    "type": "object", 

    "required": ["name"], 

    "properties": { 

        "id": { 

            "type": "integer", 

            "description": "The ID of the text set.", 

            "readOnly": true 

        }, 

        "name": { 

            "type": "string", 

            "description": "The name of the text set." 

        }, 

        "description": { 

            "type": "string", 

            "description": "The description about the text set." 

        }, 

        "created_at": { 

            "type": "string", 

            "description": "The time when the text set record is created in the system.", 

            "readOnly": true 

        }, 

        "updated_at": { 

            "type": "string", 

            "description": "The last time when the text set record is updated in the system.", 

            "readOnly": true 

        }, 

        "texts": { 

            "type": "array", 

            "items": {"$ref": "/api/v1/schemas/text.schema.json"} 

        } 

    } 

}
```

### Word Frequency option

The settings of running word frequencies. 

#### JSON schema

```
{ 

    "$id": "https://intelligentarchive.sydney.edu.au/api/v1/schemas/wfoption.schema.json", 

    "$schema": "https://json-schema.org/draft/2020-12/schema", 

    "title": "Word frequency options", 

    "description": "The settings when running word frequencies.", 

    "type": "object", 

    "properties": { 

        "segmentByCharacter": { 

            "type": "boolean", 

            "description": "Segment the text by speach characters.", 

            "default": false 

        }, 

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

        "numberOfNGrams": { 

            "type": "integer", 

            "description": "The number of n-grams used for frequencies.", 

            "exclusiveMinimum": 0, 

            "default": 1 

        }, 

        "useOriginalSpellReg": { 

            "type": "boolean", 

            "description": "Use the regularized form of words in TEI (wrapped in <reg>). Set to false to use the original form.", 

            "default": false 

        }, 

        "useOriginalSpellCorr": { 

            "type": "boolean", 

            "description": "Use the correction form of words in TEI (wrapped in <corr>). Set to false to use the original form.", 

            "default": false 

        }, 

        "countGraphemes": { 

            "type": "boolean", 

            "description": "Count graphemes for frequencies instead of words.", 

            "default": false 

        }, 

        "outputSpecialWords": { 

            "type": "array", 

            "description": "A list of special words used for output.", 

            "items": { 

                "type": "string" 

            }, 

            "default": [] 

        }, 

        "outputSpecialWordsOption": { 

            "type": "integer", 

            "description": "The way of handling special words.", 

            "default": 0, 

            "oneOf": [ 

                { 

                    "const": 0, 

                    "description": "Count the frequencies for all words but only ouput the frequencies of special words." 

                }, 

                { 

                    "const": 1, 

                    "description": "Only count and ouput the frequencies of special words." 

                }, 

                { 

                    "const": 2, 

                    "description": "Count the frequencies for all words but only ouput the frequencies of words which are not included in the special words." 

                } 

            ] 

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
```
