# SwissCovid Check-in QR-Code format

The SwissCovid Check-in QR-Code format is based on the CrowdNotifier QR-Code format as describe in the [CrowdNotifier Tech Spec](https://crowdnotifier.readthedocs.io/en/latest/basic.html#entry-qr-code-format). The QRCodePayload protobuf is base64 encoded and added to a url of the form `https://qr.swisscovid.ch?v=4#<base64-encoded-protobuf>`.

## CountryData

The `QRCodePayload` format outline in the CrowdNotifier technical specification leaves room for country-specific data in the `countryData` field. SwissCovid uses this field to add SwissCovid-specific data to the QR code. In particular, `countryData` contains the protobuf encoding of the following data structure:

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

The venue type is used to distinguish two types of venues. For a venue type `USER_QR_CODE` the SwissCovid app implements [Server-Based CrowdNotifier](https://crowdnotifier.readthedocs.io/en/latest/server-based.html). For these venues, a individual user that is present and is later diagnosed positive can themselves trigger notifications after entering a valid CovidCode. This is implemented in SwissCovid 2.0.

The venue type CONTACT_TRACING_QR_CODE would be used for bigger events, where a health authority decides when users should be notified as described in [Basic CrowdNotifier](https://crowdnotifier.readthedocs.io/en/latest/basic.html). This is not yet implemented in SwissCovid 2.0.

### Checkout Delays

The value `automaticCheckoutDelaylMs` determines the maximum check-in duration for each venue. It is not possible to be checked in longer than this duration. If the user did not check out before the maximum duration is over, the app will automatically do so. To warn the user that there is an automatic checkout coming up, after `checkoutWarningDelayMs` the app shows a notification to users to remind them to check out. Both those configuration values are in milliseconds. The value `checkoutWarningDelayMs` must be smaller than `automaticCheckoutDelaylMs`. When generating QR-Codes from the SwissCovid app the following default values are used:
- `checkoutWarningDelayMs = 1000 * 60 * 60 * 8` (8 hours)
- `automaticCheckoutDelaylMs = 1000 * 60 * 60 * 12` (12 hours)

### Reminder Options

Upon check-in the user can choose to get a reminder notification on when to check-out. The options the user can choose from are encoded in the `reminderDelayOptionsMs`. The app will always show an 'OFF' option and the first three values of `reminderDelayOptionsMs` that are larger than zero as well as an option for a custom reminder time. If the filtered array of `reminderDelayOptionsMs` is empty the default options are used. These default options are also set when generating a QR-Code from the SwissCovid app. The default reminder options are:
- `1000 * 60 * 30` (30 min)
- `1000 * 60 * 60` (1h)
- `1000 * 60 * 60 * 2` (2h)
