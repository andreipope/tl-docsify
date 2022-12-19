# Versioning

The Twelve Labs Video Understanding API is versioned. You specify the version by including it into the URL: `https://https://api.twelvelabs.io./{version}/{resource}/{path_parameters}?{query_parameters}`.

The following example URL
## Changes by Version
## v1.1
### New Features

The version 1.1 of the Twelve Labs Video Understanding API introduces the following new features:

- **[Experimental Feature] Classification of content:** You can now define a list of labels that you want to classify your videos into, and the new classification API endpoint will return the the duration for which the specified labels have appeared in your videos and the confidence that each of the matches represents the label you've specified.
- **[Experimental Feature] Query builder:** The Twelve Labs Video Understanding APO now allows you to construct complex queries such as:
  - **Negative(NOT) Search**: In addition to the existing `AND` operator, you can now construct a query something like a scene where “cooking” is happening but **EXCLUDING(NOT)**” spaghetti” scenes.
  - **Time-Proximity Search**: You can now construct a query like a scene where a “car accident” happened **[10 seconds before (Proximity)]** “winning the race.”
- **Modules**: A module is an array that contains confidence level of each search option module. This is useful when the response contains intersection results. **TODO: Is this still a thing?**
- **Metadata**: To help you categorize your videos, the API service now allows you to add custom metadata to your videos. For details, see the [Update Video Information](/reference/updatevideoinformation) page.

### Differences

This section lists the differences between the version 1 and version 1.1 of the Twelve Labs Video Understanding API.

- When you make an API call, make sure that you specify the `1.1` version. The URL should look similar to the following one: `https://https://api.twelvelabs.io./v1.1/{resource}/{path_parameters}?{query_parameters}`. For details, see the [Call an Endpoint]() section
- The following methods now return a `200 OK` status code when the response is empty:
  - `[GET] /indexes`
  - `[GET] /tasks`
  - `[GET] /indexes/{index_id}/videos`

- The `/tasks` endpoint is now a separate endpoint and is no longer part of the `/indexes` endpoint. The table below shows the changes made to each method of the `/tasks` endpoint:

  | 1.0         | 1.1         |
  | ----------- | ----------- |
  | GET `/indexes/tasks` | GET `/tasks` |
  | POST `/indexes/tasks` | POST `/tasks` |
  | GET `/indexes/tasks/{task_id}` | GET `/tasks/{task_id}` |
  | DELETE `/indexes/tasks/{task_id}` | DELETE `/tasks/{task_id}` |
  | POST `/indexes/tasks/transfers` | POST `/tasks/transfers` |
  | GET `/indexes/tasks/status` | GET `/tasks/status` |

- The `/indexes/tasks/{task_id}/video_id` endpoint has been deprecated. You can now retrieve the unique identifier of a video by invoking the GET method of the `/tasks/{task_id}` endpoint. The response will contain a field named `video_id.`.

- When an error occurs, the API service now follows the recommendations of the [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110.html) standard. Instead of numeric codes, the API service now returns string values containing human-readable descriptions of the errors. The format of the error messages is as follows:
  - `code`: A string representing the error code.
  - `message`: A human-readable string describing the error, intended to be suitable for display in a user interface.
  - `docs_url`: The URL of the relevant documentation page.

  For more details, see [Error Codes](/reference/error-codes) page.

- The `next_page_id` and `prev_page_id` fields of the `page_info` object have been renamed to `next_page_token` and `prev_page_id.`

- The `type` field has been removed from all the responses.

- Only the `GET` and `POST` methods respond with `response.data`.