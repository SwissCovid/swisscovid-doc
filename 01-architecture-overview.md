<h1>SwissCovid Architecture</h1>

The SwissCovid app consists of several frontend and backend components:

- SwissCovid app: The apps running on users iOS or Android devices.
- SwissCovid red backend: The backend for uploading/providing the list of TEKs for proximity tracing.
- SwissCovid purple backend: The backend for uploading/providing the list of exposed events for presence tracing.
- CovidCode backend (black backend): The backend for issuing and validating Covidcodes.
- CovidCode UI: The web frontend for generating Covidcodes (used by health authorities).
- Config Backend: Backend component that serves configuration values for the SwissCovid app.
- Additional Info Backend: Backend component that serves the data for the statistics tab in the SwissCovid app.  

A detailed overview of the SwissCovid architecture can be found on [https://github.com/admin-ch/PT-System-Documents](https://github.com/admin-ch/PT-System-Documents/blob/master/overview.md).