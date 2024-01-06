# Implementation notes

These are purely developer-oriented notes that highlight the differences between the Outlook <-> Google sync flow and the original Outlook <-> Outlook sync flow.

- Some fields just have different names:
  - `iCalUId` in Outlook is `iCalUID` in Google, but we should not use it, because for _recurring_ events, Google's `iCalUID` value is always the same.
  - `subject` in Outlook is `summary` in Google
  - `body` in Outlook is `description` in Google
  - Regarding the _attendees_, Outlook has `requiredAttendees` and `optionalAttendees`, whereas Google has only `attendees`.
- Google's events have a `"status": "confirmed"` field whose meaning is unclear. It is _not_ the response type. I never managed to set it to `tentantive`, which is an allowed value. It seems to be the status of the meeting as defined by the _organizer_. It may (according to the docs) also have the value `cancelled`, which is pointless, because cancelled events are typically deleted and not returned by neither the Google nor the Outlook APIs.
- While Google's Create/Update blocks allow to set an `isAllDay` property, the response of the List block lacks such a property. Instead, you can derive whether a Google event is “all day” by looking at the start/end timestamp: in this case, it is just something like "2024-01-12" (the time is missing).
- The events of both APIs have a `start` and `end` field, but their content is different:
  - Outlook: "2024-01-06T12:30:00.0000000" (implicitly UTC, but lacks a timezone specifier)
  - Google: "2024-01-05T19:15:00+00:00" (also UTC, which is explicitly stated)
  - To compare two events (one from Google, one from Outlook), we need to convert the Outlook event to UTC, and then compare the two timestamps: we call `formatDateTime(<reference-to-datestring>, ‘yyyy-MM-ddTHH:mm:ss’)` - this function seems to be able to parse ISO-like-formatted strings and reformat them using our own format (which strips timezone information or any subsecond parts, which are not needed for the synchronization anyway)
  - When we want to create an event in Google based on an Outlook event (or vice versa), we _must_ call `formatDateTime(<reference-to-datestring>)` to ensure that we always have a timezone specifier, otherwise we cannot even save the flow and get an error such as this: `The parameter with value '"@outputs('Get_calendar_view_of_events_(V3)')?['body/value'][0]['start']"' in path 'newEvent/start' with type/format 'String/date-no-tz' is not convertible to type/format 'String/date-time'`.
- The `id` field of _recurring_ Google events has the following form: `"id": "03qn2lhhkt46jbtqbtefjjfukl_20240106T170000Z"`, i.e., it contains the datetime of the recurrence. This means that the IDs of the individual event instances are _not_ stable. Fortunately, our sync flow can handle this situation, as it simply creates new SyncBlocker events and deletes the outdated ones.
