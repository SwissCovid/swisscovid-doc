<h1>SwissCovid Check-in QR-Code format</h1>

The SwissCovid Check-in QR-Code format is based on the CrowdNotifier QR-Code format as describe in the [CrowdNotifier Tech Spec](https://crowdnotifier.readthedocs.io/en/latest/basic.html#entry-qr-code-format). The QRCodePayload protobuf is base64 encoded and added to a url of the format 'https://qr.swisscovid.ch?v=4#<base64-encoded-protobuf>'.

## CountryData

The countryData bytes of the CrowdNotifier QR-Code format are also filled with a protobuf that has the following format:

```
message SwissCovidLocationData {
  uint32 version = 1;
  VenueType type = 2;
  string room = 3;
  int64 checkoutWarningDelayMs = 4;
  int64 automaticCheckoutDelaylMs = 5;
  repeated int64 reminderDelayOptionsMs = 6;
}

enum VenueType {
  USER_QR_CODE = 0;
  CONTACT_TRACING_QR_CODE = 1;
}
```

### VenueType

The venue type is used to distinguish two types of venues. For a venue type USER_QR_CODE the SwissCovid app implements the [Server-Based CrowdNotifier](https://crowdnotifier.readthedocs.io/en/latest/server-based.html) where an individual user present at a venue can trigger a notification by entering the covidcode in the app. This is implemented in SwissCovid 2.0.

The venue type CONTACT_TRACING_QR_CODE would be used for bigger events, where a health authority decides when users should be notified as described in [Basic CrowdNotifier](https://crowdnotifier.readthedocs.io/en/latest/basic.html). This is not yet implemented in SwissCovid 2.0.

### Checkout Delays

With `automaticCheckoutDelaylMs` the maximum check-in duration can be defined for each venue. It is not possible to be checked in longer than this duration and if the user did not check-out, the app will automatically check-out the user. To warn the user, that there is an automatic checkout coming up, after `checkoutWarningDelayMs` a notification is shown to the user that reminds to check-out. Both those configuration values need to be provided in milliseconds and `checkoutWarningDelayMs` must be smaller than `automaticCheckoutDelaylMs`. When generating QR-Codes from the SwissCovid app the following default values are used:
- `checkoutWarningDelayMs = 1000 * 60 * 60 * 8` (8 hours)
- `automaticCheckoutDelaylMs = 1000 * 60 * 60 * 12` (12 hours)

### Reminder Options

Upon check-in the user can choose to get a reminder notification on when to check-out. The options the user can choose from are encoded in the `reminderDelayOptionsMs`. The app will always show an 'OFF' option and the first four values of `reminderDelayOptionsMs` that are larger than zero. If the filtered array of `reminderDelayOptionsMs` is empty the default options are used. These default options are also set when generating a QR-Code from the SwissCovid app. The default reminder options are:
- `1000 * 60 * 30` (30 min)
- `1000 * 60 * 60` (1h)
- `1000 * 60 * 60 * 2` (2h)
- `1000 * 60 * 60 * 4` (4h)