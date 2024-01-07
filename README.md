# outlook-google-calendar-sync
A Microsoft Power Automate flow to synchronize an Outlook 365 with a Google calendar.

In contrast to Desktop solutions such as https://www.outlookgooglecalendarsync.com, this solution runs in the cloud and does not require a Desktop PC to be running.

This is a fork of the [outlook-calendar-sync](https://github.com/MShekow/outlook-calendar-sync) project, which synchronizes two Outlook 365 calendars.

**Note: this Outlook <-> Google sync flow is not battle-tested yet!!**

Download the zip archive from [here](https://github.com/MShekow/outlook-google-calendar-sync/raw/main/Outlook%20Google%20calendar%20sync%20v0.8.zip).

If you want to get rid of all blocker events, you can use [this](https://github.com/MShekow/outlook-google-calendar-sync/raw/main/Delete%20SyncBlocker%20events.zip) helper PowerAuto flow.

## Limitations

The Google calendar API (and/or the corresponding Power Automate action) has various limitations:

- The Google APIs do not seem to expose the _visibility_ (e.g. public, private), _response type_ (e.g. tentative), _reminders_, or the _show as_ (busy, free) attributes. Thus, we cannot synchronize them to Outlook. Conversely, we also cannot set these attributes when creating/updating Google calendar events.
- The Google APIs do not expose the _timezone_ of the events, thus we always set UTC for Outlook calendar events.
- The Google APIs do not expose the _color_ that you can assign to events in the Google calendar web interface or app.

Some limitation details are also discussed in the [implementation notes](implementation-notes.md).

## Change log

### v0.8 (2024-01-06)

* Initial release, based on version 0.8 of the `outlook-calendar-sync`
