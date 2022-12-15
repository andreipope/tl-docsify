# `/experimental/search`

Use the `/search` endpoint to search for relevant matches in an index. This endpoint supports pagination and filtering. 


NOTE: When you use pagination, you will not be charged for retrieving subsequent pages of results.


## Make a Search Request

Method: POST `/search`

Use this endpoint to search for relevant matches in an index. This endpoint returns the first page of results. To retrieve the subsequent pages, you must call the POST method of the `/search/{page-id}` endpoint, passing it the unique identifier of the page you want to retrieve.

NOTE: When you use pagination, you will not be charged for retrieving subsequent pages of results.

### Body Parameters 

#### filter 

- Type: `object`
- Default: N/A
- Description:

Use this parameter to filter your search results by metadata.

For string fields, the filter parameter returns only the results that equal the value you specify. The following example filters on the videos whose title is "Animal Encounters part 1": "title": "Animal Encounters part 1".

For numeric fields, the filter parameter returns the results that match based on the arithmetic comparison. The following example filters on the videos whose height is greater than or equal to 400 and less than or equal to 500: "height": { "gte": 400, "lte": 500 }.

You can also filter by any of the custom fields specified by invoking the PUT method of the /indexes/{index-id}/videos/{video-id} endpoint. The following example returns only the videos for which a custom field named needsReview of type boolean is set to true: "needs_review": true.

#### group_by

- Type: `string`
- Possible values: `video`, `clip`
- Default: `clip`
- Description:

TODO


#### index_id

- Type: `string`
- Default: N/A
- Description:

The unique identifier of the index to search.

#### page_limit

- Type: `number`
- Default: `10`
- Max: `50`
- Description:

The number of items to return on each page.


#### sort_option

- Type: `string`
- Possible values: `score`, `clip_count`
- Default: `score`
- Description:

Use the `sort_option` parameter to specify how the API service should sort the results. The following options are available:

- `score`: When the `group_by` parameter is set to `video`, the results are sorted by the maximum score of the clips in each video.
- `clip_count`: You can use this value when the `group_by` parameter is set to `video`. The API service will return the results sorted by the number of matching fragments in each video.  

#### threshold

- Type: string
- Possible values: `high`, `medium`, `low`, `none`
- Default: `low`
- Description:

This parameter represents the miniumum similarity score between your search terms and the matching video fragments. 


#### query

- Type: `object`
- Default: N/A
- Description:

TODO

Your search query, represented as an array of key-value pairs, where key name is the operator that you want to use.

The operator can take one of the following values:

- `$and`: Represents the logical `AND` operator. The API service will return the video fragments for which all the queries match.
- `$or`:Represents the logical `AND` operator. The API service will return the video fragments for which all the queries match.
- `$not`: TODO
- `$strict_or`: TODO

The value is an object that has the following fields:

- `text` - A string containing your search query
- `option` - See `query.option`. Not sure where to place this. TODO


```
      query:
        additionalProperties:
          type: string
        description: |-
          Combination of queries with using operators

          Query: `Block` | `Unit`
          - Block: { "$and": [`Query`] } | { "$or": [`Query`] } | { "$not": `Query` } | { "$strict_or": [`Query`] }
            - "$not" can be valid only when other queries are in the same block
            - "$strict_or" is a special operator which shows the results only when it follows the order of queries
          - Unit : { "text": string, "option": "conversation" | "text_in_video" | "visual", "conversation_option": "transcription" | "semantic" }
            - "text" and "option" are required
            - "conversation_option" is required when "option" is "conversation". Default is "semantic"

          e.g.
          ```json
          { "text": "A man holding an iPad", "option": "visual" }
          ```
          e.g.
          ```json
          {
            "$and": [
              { "text": "A man holding an iPad", "option": "visual"},
              { "text": "iPad", "option": "text_in_video"},
              { "$not": { "text": "Samsung galuxy", "option": "conversation" }
            ]
          }
          ```
        example:
          option: visual
          text: A man holding an iPad
        type: object
```

Notes: 
- The API supports full natural language-based search. The following examples are valid queries: "birds flying near a castle", "sun shining on water", "chickens on the road", "an officer holding a child's hand.", "crowd cheering in the stadium."
- Search operator $not is valid only when wrapped with $and operator and has more than one query in a same block.


#### query.adjacency_threshold

- Type: number 
- Default: 0
- Description:

When specifying multiple search queries, you can use the `adjacency_threshold` to limit or expand your search results by extending the lower and upper boundaries of each matching video fragment. This parameter is expressed in seconds.


Example:
You've specified two queries and used to `AND` logical operator to retrieve only the video fragment for which both queries match. The first query matches a video fragment that starts and 10s and end at 10s. The second query second query matches a video fragment that starts at 50s and ends at 70s


If you don't specify the `adjacency_threshold` parameter, then it takes the default value of 0s. The video fragments will not overlap, and the API service will not return any results.

If you specify a value of 30s for the `adjacency_threshold` parameter, the first video fragment will start at 0s and end at 40s, and the second video fragment will start at 20s and end at 100s. The video fragments will overlap, and the API service will return a result that starts 20s and ends at 40s.


#### query.conversation_option

- Type: `string`
- Possible values: `transcription`, `semantic`
- Default: `semantic`
- Description:

Specifies the type of search the API service will perform. The following values are supported:

- `semantic`: The API service returns results that are semantically similar to the search query even if the results don't exactly match the search query.
- `transcription`: The API service returns only the exact matches. Note that transcription can only be used with a single search option. For example, you cannot set the `search_option` parameter to equal `["visual", "conversation", "text_in_video"]` and the `conversation_option` parameter to equal `transcription`.

#### query.option

- Type: `array` of strings
- Possible values: subset of `["visual", "conversation", "text_in_video"]`
- Default: N/A
- Description:

Search options specify the source of information the API service uses when performing a search. The following values are supported:

- `visual`: Allows you to search by objects, actions, sound, movements, places, situational events, and complex audio-visual text descriptions.
- `conversation`: Allows you to find the exact point in your video where the specified word or phrase is mentioned.
- `text_in_video`: Allows you to search for text that appears in your videos (OCR).
The search options you specify must be a subset of the indexing options used when you created the index.
You can specify multiple search options in conjunction with the operator parameter to broaden or narrow your search.

#### query.order_strict

- Type: boolean
- Possible values: `false`, `true`
- Default: `false`
- Description:


TODO

(1) query A and query B is in order so it will aggregated into single response but (2) query A and query B is not in order so it will not included.
For now, we support order_strict only for the $or operator. But $and will be supported as well in few weeks 


### Response
