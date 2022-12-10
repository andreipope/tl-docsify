# Migration Guides

## Considerations

## Migrate from V1 to V1.1

This migration guide helps you migrate from version 1 to version 1.1 of the Twelve Labs Video Understanding API.

### New Features (Benefits???)

The version 1.1 of the Twelve Labs Video Understanding API introduces the following new features:

- **[Experimental Feature] Classification of content:** You can now define a list of contexts that you want to classify the video into, and the new classification API endpoint will return how long (duration) and strong (score) each context has appeared in the video. For details, see the **TODO** page. 
- **[Experimental Feature] Query builder:** The Twelve Labs Video Understanding API has now been updated to include a new query-building functionality that allows you to perform the following new types of search:
  - **Negative(NOT) Search**: In addition to the existing `AND` operator, you can now construct a query something like a scene where “cooking” is happening but **EXCLUDING(NOT)**” spaghetti” scenes.
  - **Time-Proximity Search**: You can now construct a query like a scene where a “car accident” happened **[10 seconds before (Proximity)]** “winning the race.”
- **Modules**: is an array that contains confidence level of each search option module. This is useful when the response contains intersection results. **TODO: See if this is specific to v1.1**
- **Metadata**: You can now set and get your own metadata on each video. It can be set after status of task turned into `ready`, and only accessible through `video` endpionts. Custom metadata supports `string`, `integer`, `float`, and `boolean`. If you need to store other types of data, please do stringify to save and parse it on your system. Also, some pre-defined fields like `width` and `size` are not allowed to override. If you try so, it would return a `403 Forbidden` error. **TODO: See if this is specific to v1.1**

### Differences

This section list the differences between the version 1 and version 1.1 of the Twelve Labs Video Understanding API.

- **Specifying the version:** When you make an API call, make sure that you specify the 1.1 version. The URL should look similar to the following one: `https://https://api.twelvelabs.io./v1.1/{resource}/{path_parameters}?{query_parameters}`. For details, see the [Call an Endpoint]() section
- So in API v1.1, we return a `200 OK` status code even if response data is empty. Affected endpoints are listed below.

1. `[GET] /indexes`
2. `[GET] /tasks`
3. `[GET] /indexes/:index_id/videos`

