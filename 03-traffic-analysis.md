# SwissCovid Traffic Analysis Protection

Adversaries that can observe network communication can observe network traffic between a user’s smartphone and the backend server(s). Powerful network adversaries might also be able to observe network traffic between backends. Network adversaries can use these observations to try to infer sensitive information about users: whether they received a positive COVID-19 diagnosis, or whether they received a notification of exposure to a COVID-19 positive users.

SwissCovid notifications do not generate traffic, and therefore are invisible to network adversaries. 

To prevent inferences about positive diagnosis, the SwissCovid app periodically sends out dummy (fake) requests to the backend servers that look like actual requests, see [the DP-3T Best Practices document](https://github.com/DP-3T/documents/blob/master/DP3T%20-%20Best%20Practices%20for%20Operation%20Security%20in%20Proximity%20Tracing.pdf) for a high-level introduction. For SwissCovid v2.0, to ensure fake requests look like real requests, fake requests must simulate the redemption of a CovidCode at the black server, and the subsequent upload to the red and purple servers. 

Servers pad their computation time to ensure that the time to process fake redemption requests and uploads is not distinguishable from the time required to process real ones. 

Network observers can also observe the time between redeeming the CovidCode and making the uploads. In SwissCovid v2.0 there might be considerable delay between redeeming and uploading due to user interactions (e.g., approving the upload or selecting check-ins). Dummy requests must generate similar delays. In order to learn the best delay distribution, the current version of the app collects anonymized samples of the user interaction time with the app. 

To ensure that the real distribution is not too erratic, the SwissCovid app waits a uniformly random time of 5 seconds after all user actions are completed before making the actual uploads. Dummy requests instead wait between 0 and 3 minutes between redeeming a code and making the uploads. The fact that the real distribution is somewhat smoothed due to the extra delay, and the ratio of dummy uploads and real uploads is very high, ensures that users have good protection despite these distributions not matching.

A detailed description and analysis of these processes can be found [in the accompanying notebook](swisscovid/dummy-analysis.ipynb).
