<h1 align="center">SwissCovid Documentation</h1>

This repository serves as an entry point for the documentation of SwissCovid. The SwissCovid 2.0 app uses two types of contact tracing to prevent the spread of COVID-19.

 * *Proximity Tracing*. SwissCovid uses proximity tracing to notify users when they have been close to a COVID-19 positive person. Proximity tracing uses Bluetooth technology to estimate distance. SwissCovid builds on the [DP3T SDK](https://github.com/DP-3T/documents), which in turn uses the [Google & Apple Exposure Notifications framework](https://www.google.com/covid19/exposurenotifications/). This feature is called _SwissCovid Encounters_.
 
 * *Presence Tracing*. SwissCovid uses presence tracing to notify users when they have been at the same location/venue at the same time as a COVID-19 positive person. Presence tracing builds on the [CrowdNotifier SDK](https://github.com/CrowdNotifier/documents) to provides a secure, decentralized, privacy-preserving presence tracing. This feature is called _SwissCovid Check-in_.

## Content
- [SwissCovid Components](01-architecture-overview.md)
- [Covidcode](02-covidcode.md)
- [Traffic analysis resistance](03-traffic-analysis.md)

#### CrowdNotifier
- [QR-Code format](crowdnotifier/02-qrcode-format.md)
- [Background synchronisation](crowdnotifier/03-background-synchronisation.md)

## Repositories SwissCovid
* Documentation Overview: [swisscovid-doc](https://github.com/SwissCovid/swisscovid-doc)
* Android App: [swisscovid-app-android](https://github.com/SwissCovid/swisscovid-app-android)
* iOS App: [swisscovid-app-ios](https://github.com/SwissCovid/swisscovid-app-ios)
* UX & Screenflows
[swisscovid-ux-screenflows](https://github.com/SwissCovid/swisscovid-ux-screenflows)
* CovidCode Web-App: [CovidCode-UI](https://github.com/admin-ch/CovidCode-UI)
* CovidCode Backend: [CovidCode-Service](https://github.com/admin-ch/CovidCode-service)
* Config Backend: [swisscovid-config-backend](https://github.com/SwissCovid/swisscovid-config-backend)
* Additional Info Backend: [swisscovid-additionalinfo-backend](https://github.com/SwissCovid/swisscovid-additionalinfo-backend)
* QR Code Landingpage: [swisscovid-qr-landingpage](https://github.com/SwissCovid/swisscovid-qr-landingpage)

## DP^3T
The Decentralised Privacy-Preserving Proximity Tracing (DP-3T) project is an open protocol for COVID-19 proximity tracing using Bluetooth Low Energy functionality on mobile devices that ensures personal data and computation stays entirely on an individual's phone. It was produced by a core team of over 25 scientists and academic researchers from across Europe. It has also been scrutinized and improved by the wider community. The core of the design of SwissCovid and some of its protections follow closely the recommendations of the DP-3T project.

DP^3T is a free-standing effort started at EPFL and ETHZ that produced this protocol and that is implementing it in an open-sourced app and server.

### Repositories
* Documents: [dp3t-documents](https://github.com/DP-3T/documents)
* Android SDK & Calibration app: [dp3t-sdk-android](https://github.com/DP-3T/dp3t-sdk-android)
* iOS SDK & Calibration app: [dp3t-sdk-ios](https://github.com/DP-3T/dp3t-sdk-ios)
* Backend SDK: [dp3t-sdk-backend](https://github.com/DP-3T/dp3t-sdk-backend)

## CrowdNotifier
CrowdNotifier is a protocol for building secure, decentralized, privacy-preserving presence tracing systems. It simplifies and accelerates the process of notifying individuals that shared a semi-public location with a SARS-CoV-2-positive person for a prolonged time without introducing new risks for users and locations. Proximity tracing systems (such as the Encounter functionality in SwissCovid) notify only a subset of these people: those that were close enough for long enough.

The CrowdNotifier design aims to minimize privacy and security risks for individuals and communities and limit the possibility of abuse, while guaranteeing the highest level of data protection and good usability and deployability. For further details on the design, see the [CrowdNotifier technical specification](https://crowdnotifier.readthedocs.io/).

### Repositories
* Documents: [crowdnotifier-documents](https://github.com/CrowdNotifier/documents)
* Technical Specification: [crowdnotifier-techspec](https://crowdnotifier.readthedocs.io)
* Android SDK: [crowdnotifier-sdk-android](https://github.com/CrowdNotifier/crowdnotifier-sdk-android)
* iOS SDK: [crowdnotifier-sdk-ios](https://github.com/CrowdNotifier/crowdnotifier-sdk-ios)
* Backend: [swisscovid-cn-backend](https://github.com/SwissCovid/swisscovid-cn-backend)

## License
This project is licensed under the terms of the MPL 2 license. See the [LICENSE](LICENSE) file.
