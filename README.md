# outlook-google-calendar-sync
A Microsoft Power Automate flow to synchronize an Outlook 365 with a Google calendar.

In contrast to Desktop solutions such as https://www.outlookgooglecalendarsync.com, this solution runs in the cloud and does not require a Desktop PC to be running.

This is a fork of the [outlook-calendar-sync](https://github.com/MShekow/outlook-calendar-sync) project, which synchronizes two Outlook 365 calendars.

**Note: this Outlook <-> Google sync flow is not battle-tested yet!!**

Download the zip archive from [here](https://github.com/MShekow/outlook-google-calendar-sync/raw/main/Outlook%20Google%20calendar%20sync%20v0.9.zip).

If you want to get rid of all blocker events, you can use [this](https://github.com/MShekow/outlook-google-calendar-sync/raw/main/Delete%20SyncBlocker%20events.zip) helper PowerAuto flow.

## Limitations

The Google calendar API (and/or the corresponding Power Automate action) has various limitations:

- The Google APIs do not seem to expose the _visibility_ (e.g. public, private), _response type_ (e.g. tentative), _reminders_, or the _show as_ (busy, free) attributes. Thus, we cannot synchronize them to Outlook. Conversely, we also cannot set these attributes when creating/updating Google calendar events.
- The Google APIs do not expose the _timezone_ of the events, thus we always set UTC for Outlook calendar events.
- The Google APIs do not expose the _color_ that you can assign to events in the Google calendar web interface or app.
- The _attendees_ are not synchronized, because the synchronization algorithm uses that field to correlate events, i.e., the ID of the source event is stored as an attendee in the target / SyncBlocker event.

Some limitation details are also discussed in the [implementation notes](implementation-notes.md).

## Change log

### v0.9 (2024-03-02)

* **WARNING**: if you are upgrading from 0.8 or older, **you must first run the _Delete SyncBlocker events_ flow** to delete all old SyncBlocker events. You must run the _Delete SyncBlocker events_ flow once for every calendar that is affected by your v0.8 (or older) synchronization flow.
  * Note: the zip archive of the _Delete Syncblocker events_ flow has been updated, so that it can delete SyncBlocker events created with v0.9 or newer. If you run into problems with v0.9 and want to revert to v0.8, make sure that you download the new version of the _Delete SyncBlocker events_ flow (and **re**-import it in case you already had the old version), to properly clean your SyncBlocker events.
* The _location_ field is now synchronized. In v0.8 and older, the location field of the SyncBlocker events used to store the ID of the source event. In v0.9, that ID is now stored in a fake email address in the first required _attendee_ of the SyncBlocker event. Make sure not to delete this attendee!
* The _SyncBlocker prefix_ is now optional, because the synchronization algorithm now disambiguates real events from SyncBlocker events via the _required attendees_ field. If you want, you can now set the SyncBlocker prefix to an empty string, and instead use a _category_ to visually disambiguate your real vs. blocker events in Outlook. You may now also change the SyncBlocker prefix _at any time_, the flow will automatically update the subject/title of all events accordingly (in v0.8 and older, you were not allowed to change the prefix after running the first sync).
* For already-synchronized events, changes made to the title / subject of the source event are now propagated to the SyncBlocker event.

### v0.8 (2024-01-06)

* Initial release, based on version 0.8 of the `outlook-calendar-sync`
