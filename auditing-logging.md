---

copyright:

  years: 2015, 2017, 2018

lastupdated: "2019-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}    

# Auditing and logging
{: #auditing-logging}

The auditing and logging capabilities in {{site.data.keyword.cfee_full}} (CFEE) allow administrators to audit events taking place in a CFEE instance, and developers to track log events generated from their Cloud Foundry applications.

Auditing and logging in CFEE are suppoted through integration with the Activity Tracker and LogDNA services instantiated in the IBM Cloud.


## Auditing
{: #auditing}

The Activity Tracker service has been **deprecated** in the IBM Cloud. Consequently, new instances of the Activity Tracker service cannot be created to enable auditing in a CFEE. Auditing enabled in a CFEE through pre-existing Activity Tracker instances will continue to work. 
{: important}

Auditing allows CFEE administrators to track Cloud Foundry auditable activities taking place in a CFEE instance.  Those activities include login, creation of organizations and spaces, user membership and role assignments, application deployments, service bindings, and domain configuration. Auditing is supported through integration with the Activity Tracker service in the IBM Cloud. An instance of the Activity Tracker service selected by the CFEE administrator is configured automatically to receive events representing actions performed within Cloud Foundry and on the CFEE control plane.  The user can see and manage those events in the user interface of the Activity Tracker service instance.

To enable auditing for a CFEE instance:

1. Open a CFEE's user interface and to **Operations > Auditing** entry in the left navigation pane to open the Logging page.
2. Click **Enable auditing** and select one of the **Activity Tracker instances** available in the IBM Cloud account.  
3.  Once auditing is enabled, configuration details are displayed on the page. Details include the status of the configuration, and a link to the Activity Tracker service instance itself, where the user can go to see and manage auditing events.

You can disable Auditing by clicking **Disable auditing**, which will remove the Activity tracker service instance previously added and configured. This action does not delete the Activity Tracker service instance.


## Logging persistence
{: #logging}

The Log Analysis service has been **deprecated** in the IBM Cloud. Logging persistence in CFEE will be supported through integration with the LogDNA service starting in CFEE v3.1.0 (replacing the deprecated Log Analysis service). Logging persistence enabled through integration with pre-existing Log Analysis instances will continue to work even after the CFEEE instance is updated to v3.1.0.  However, once logging persistence enabled though a pre-existing Log Analysis service instance is **disabled**, re-enablement can take place only through a LogDNA instance.
{: important}

Logging of Cloud Foundry events is supported through integration with the LogDNA service in the IBM Cloud. An instance of the LogDNA service selected by the CFEE administrator is configured automatically to receive and persist Cloud Foundry logging events generated from the CFEE instance.  The user can see and manage those logging events in the user interface of the LogDNA service instance.

To enable logging for a CFEE instance:

1. Make sure that you have an [IAM access policy](https://cloud.ibm.com/iam/#/users) that assigns you either administrator platform role, or viewer platform role with reader role in the LogDNA service instance into which you intend to persist the logging events.
2. Open a CFEE's user interface and to **Operations > Logging** entry in the left navigation pane to open the Logging page.
3. Click **Enable persistence** and select one of the **LogDNA instances** available in the IBM Cloud account.  If no instances are available, the user will see an option to create an instance in the IBM Cloud catalog.
4. Once logging persistence is enabled, configuration details are displayed in the page. Details include the status of the configuration, and a link to the LogDNA service instance itself, where they user can go to see and manage logging events.

**Note:** Enabling logging persistence does not disrupt CFEE operations, nor triggers a restart of the CFEE control plane or cells.

You can disable Log persistence by clicking **Disable log persistence**, which will remove the service instance previously added and configured. This action will not delete the LogDNA service instance.

**Note:** When you disable log persistence, the Cloud Foundry logging events are still being generated, they are just not persisted outside the CFEE instance.
