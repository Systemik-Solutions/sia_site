# Texts

To analyse a work in SIA, you must first upload it as a Text or Text Set.

## Texts

### Platform Process

From the home page, select **Texts** to add a new Text or see Texts you’ve added previously.

#### Add a Text

1.	Select **Add new**.
2.	Fill out the form. The fields with red asterisks are compulsory.
3.	Upload your text file in the **File** field. Your file must be a Plain Text or TEI file. UTF-8 encoding is recommended but not required.
4.	Add as many metadata fields as you want. Relevant fields to consider might include ‘Edition’, ‘Digitisation Date’, or ‘Translator’.
5.	Click **Add**.

You will see your new Text in the Text list. Click its title to open it.

#### Edit a Text

1.	Click the three dots to the right of the Text you want to edit.
2.	Click **Edit**.
3.	Fill out the form to update its details.
4.	Click **Save**.

You can’t edit the file of a Text. Instead, delete the existing Text and add a new one using a different file.

***
### API Process

**API base path**: `/api/v1/`

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

## Text Sets

### Platform Process

For some experiments, Texts first need to be collated into a **Text Set**. From the home page, select **Text Sets** to create a Text Set or see Text Sets you’ve created previously.

#### Create a Text Set from existing Texts

1.	Select **Add new**.
2.	Give the Text Set a **Name** and **Description**.
3.	Use the selector widget to move Texts from the right window to the left window. These will be added to the Text Set.
4.	Click **Add**.

#### Import a Text Set from files

1.	Select **Import**.
2.	Give the Text Set a **Text Set Name** and, if relevant, a **Text Set Author**.
3.	In **Text Files**, select multiple Plain Text files to upload. The names of your files will be used as the names of the Texts in the Text Set, so make sure they are informative.
4.	Click **Import**.

You will see your new Text Set in the Text Set list. Click its title to open it.

#### Edit a Text Set

1.	Click the three dots to the right of the Text Set you want to edit.
2.	Click **Edit**.
3.	Fill out the form to update its details.
4.	Click **Save**.

#### Copy a Text Set

1.	Click the three dots to the right of the Text Set you want to edit.
2.	Click **Copy**.
3.	Give the copy a name and click **Copy**.

You will see your copy in the Text Set list.

***
### API Process

**API base path**: `/api/v1/`

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
