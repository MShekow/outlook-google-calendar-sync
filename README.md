# outlook-google-calendar-sync
A Microsoft Power Automate flow to synchronize an Outlook 365 with a Google calendar.

This is a fork of the [outlook-calendar-sync](https://github.com/MShekow/outlook-calendar-sync) project, which synchronizes two Outlook 365 calendars.

**This Outlook <-> Google sync flow is still in an early stage and not well-tested yet!!**

Download the zip archive from here TODO.

If you want to get rid of all blocker events, you can use this TODO helper PowerAuto flow.

## Limitations

The Google calendar API (and/or the Power Automate action) has various limitations:

- The Google APIs do not seem to expose the _visibility_ (e.g. public, private), _response type_ (e.g. tentative), or the _show as_ (busy, free) attributes. Thus, we cannot synchronize them to Outlook. Conversely, we also cannot set these attributes when creating/updating Google calendar events.
- The Google APIs do not expose the _timezone_ of the events, thus we always set UTC for Outlook calendar events.
- The Google APIs do not expose the _color_ that you can assign to events in the Google calendar web interface or app.

Some limitations are also discussed in the [implementation notes](implementation-notes.md).

## Change log

### v0.8 (2024-01-06)

* Initial release, based on version 0.8 of the `outlook-calendar-sync`
