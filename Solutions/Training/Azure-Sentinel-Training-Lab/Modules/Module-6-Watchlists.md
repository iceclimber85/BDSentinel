# Module 6 - Watchlists

#### 🎓 Level: 300 (Intermediate)
#### ⌛ Estimated time to complete this lab: 20 minutes

This module will show you how to use Azure Sentinel watchlists in event correlation and enrichment. Once created, you can use watchlists in your search, detection rules, threat hunting, and response playbooks.

#### Prerequisites
This module assumes that you have completed [Module 1](Module-1-Setting-up-the-environment.md), as the data and the artifacts that we will be using in this module need to be deployed on your Azure Sentinel instance.

### Exercise 1: Create a watchlist

You have received a message from the SOC manager, informing you about a penetration test exercise being performed over the next few weeks. Your manager also informs you that the SIEM is already seeing a bunch of incidents from IP addresses used by the penetration test team. These incidents all come from rule "High count of connections by client IP on many ports", which identifies when 30 or more ports are used for a given client IP in 10 minutes occurring on the IIS server. Your manager provides you [this CSV file](../Artifacts/Telemetry/PenTestsIPaddresses.csv) with the list of the IP addresses involved in the penetration exercise.

1. From Azure Sentinel portal, go to Watchlists menu and click *Add new*.

![watchlists](../Images/watchlists1.png)

2. In the watchlist wizard enter the following and click *Next: Source*:

    - Name: **PenTestsIPaddresses**
    - Description: **IP addresses used during penetration tests**
    - Watchlist Alias: **PenTestIPaddresses**

![watchlists](../Images/watchlists2.png)

3. Download the [CSV file](../Artifacts/Telemetry/PenTestsIPaddresses.csv) to your desktop. 

4. In the watchlist wizard, upload the file from your desktop, check the *Results Preview* and click *Next: Review and Create*.

![watchlists](../Images/watchlists3.png)

5. Click *Create* to finish the wizard.

6. You are brought back to the Watchlists screen, where you see your newly created watchlist. Click on it and then click on *View in Log Analytics*. 

![watchlists](../Images/watchlists4.png)

7. You should see the following screen. From the same logs screen you can also run *_GetWatchlistAlias*, which will return all defined watchlists.

![watchlists](../Images/watchlists5.png)

### Exercise 2: Whitelist IP addresses in the analytics rule

1. Go to *Analytics*, then *Templates* and search for "High count of connections". Select the "High count of connections by client IP on many ports" rule and click on *Create rule*.

![watchlists](../Images/watchlists6.png)

2. In the *Set rule logic* step of the wizard, expand the query window.

![watchlists](../Images/watchlists7.png)

3. Add the following KQL statement that brings the IPAddress field from the "PenTestsIPaddresses" watchlist: *let PenTestIPaddresses = _GetWatchlist('PenTestIPaddresses') | project IPAddress;*

![watchlists](../Images/watchlists8.png)

4. Now add an additional *where* statement to discard records where the client IP address (cIP field) matches one of the IP addresses in the watchlist. The statement is: *| where cIP !in (PenTestIPaddresses)*

![watchlists](../Images/watchlists9.png)

5. Continue through the wizard and save the modified rule.

![watchlists](../Images/watchlists10.png)


**Congratulations, you have completed Module 6!**. You can now continue to **[Module 7 - Threat Intelligence](./Module-7-Threat-Intelligence.md)**