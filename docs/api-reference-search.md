# `/search`

Use the `/search` endpoint to search for relevant matches in an index. This endpoint supports pagination and filtering. 


NOTE: When you use pagination, you will not be charged for retrieving subsequent pages of results.


## Make a Search Request

Method: POST `/search`

Use this endpoint to search for relevant matches in an index. This endpoint returns the first page of results. To retrieve the subsequent pages, you must call the POST method of the `/search/{page-id}` endpoint, passing it the unique identifier of the page you want to retrieve.

NOTE: When you use pagination, you will not be charged for retrieving subsequent pages of results.

### Body Parameters 

#### Filter 

- Type: `object`
- Default: N/A
- Status: stable
- Description:

Use this parameter to filter your search results by metadata.

For string fields, the filter parameter returns only the results that equal the value you specify. The following example filters on the videos whose title is "Animal Encounters part 1": "title": "Animal Encounters part 1".

For numeric fields, the filter parameter returns the results that match based on the arithmetic comparison. The following example filters on the videos whose height is greater than or equal to 400 and less than or equal to 500: "height": { "gte": 400, "lte": 500 }.

You can also filter by any of the custom fields specified by invoking the PUT method of the /indexes/{index-id}/videos/{video-id} endpoint. The following example returns only the videos for which a custom field named needsReview of type boolean is set to true: "needs_review": true.

#### index_id

- Type: `string`
- Default: N/A
- Status: stable
- Description:

The unique identifier of the index to search.

#### group_by

- Type: `string`
- Possible values: `video`, `clip`
- Default: `clip`
- Status: experimental
- Description:

TODO

#### sort_option

- Type: `string`
- Possible values: `score`, `clip_count`
- Default: `score`
- Status: experimental
- Description:

Use the `sort_option` parameter to specify how the API service should sort the results. The following options are available:

- `score`: When the `group_by` parameter is set to `video`, the results are sorted by the maximum score of the clips in each video.
- `clip_count`: You can use this value when the `group_by` parameter is set to `video`. The API service will return the results sorted by the number of matching fragments in each video.  

#### page_limit

- Type: `number`
- Default: `10`
- Max: `50`
- Status: stable
- Description:

The number of items to return on each page.

---

#### query

- Type: `object`
- Default: N/A
- Status: experimental
- Description:

Notes:

Search operator $not is valid only when wrapped with $and operator and has more than one query in a same block.


#### threshold

- Type: string
- Possible values: `high`, `medium`, `low`, `none`
- Default: `low`
- Status: Experimental
- Description:

This parameter represents the miniumum similarity score between your search terms and the matching video fragments. 


