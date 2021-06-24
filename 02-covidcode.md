# SwissCovid CovidCode

For security reasons, only index cases -- i.e., users with a positive COVID-19 diagnosis -- should be able to upload tracing information to the backend servers. SwissCovid uses one-time-use codes, called CovidCodes, to authenticate uploads by users.

The public health authorities are responsible for providing this CovidCode to every index case. The authorities can use the [CovidCode UI](https://github.com/admin-ch/CovidCode-UI) to generate CovidCodes. Normally, users should receive their CovidCode automatically. If they do not, they can also request a CovidCode by calling the hotline number provided in the app.

After receiving a CovidCode, users enter their code in the app. The app validates the code at the [CovidCode Service](https://github.com/admin-ch/CovidCode-service). If the CovidCode is valid (i.e., it did not yet expire and has not been used before) the CovidCode Service issues two upload authorization tokens. The app uses one token to upload the user’s TEKs for proximity tracing to the SwissCovid red backend. It uses the other token to upload the list of check-ins to the SwissCovid purple backend.
