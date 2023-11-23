# Concordance

A Concordance is a speedy and intuitive way of understanding how a particular word is being used in a Text. It provides a listing for all the occurrences of a target word together with the words occurring immediately before and after each instance.

An example of how you might use a Concordance is to map the occurrence of the word “think” in Shakespeare’s Hamlet. You could analyse the context of “think” throughout the play by looking at the words that occur before and after it.

## Platform Process

1.	From the home page, select **Concordance**.
2.	Choose a **Text**.
3.	Enter **Words to search for**.
4.	Use the sliders to set the number of words to show before and after the searched words.
5.	Click **Show results**.

You’ll see the results on the right. You can copy the data to your clipboard or export it as an .xlsx or .zip file.

## API Process

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
