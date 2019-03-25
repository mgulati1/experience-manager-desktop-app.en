---
title: AEM Desktop App Best Practices
seo-title: AEM Desktop App Best Practices
description: Key capabilities and recommended use of Adobe Experience Manager Desktop App
seo-description: Key capabilities and recommended use of Adobe Experience Manager Desktop App
uuid: ba8fbc74-e1ad-4085-a031-ffd317628ba6
contentOwner: asgupta
products: SG_EXPERIENCEMANAGER/6.4/ASSETS
topic-tags: administering
content-type: reference
discoiquuid: 57d5cd78-abce-4ede-a50e-7c161ddb43ae
index: y
internal: n
snippet: y
---

# AEM Desktop App Best Practices{#aem-desktop-app-best-practices}

## Overview {#overview}

Adobe Experience Manager (AEM) Desktop App links your digital asset management (DAM) solution with your desktop so you can open the files that are available in the AEM web UI directly on  desktop . If you save an asset from  desktop , it is uploaded to AEM at the appropriate location.

AEM Desktop App eliminates chances of you updating incorrect local copies or updating a wrong asset in AEM. Desktop App's easy-to-use workflow is enabled using network share technology that desktop operating systems provide.

Desktop App mounts the AEM Assets repository as a network share on  desktop . Therefore, the folders and files appear as if they were local. However, it is not recommended to carry out digital asset management operations directly from the desktop in the mounted network share in Finder or Explorer. Instead, Adobe recommends that you use AEM Assets Web UI to performs operations, such as copying or moving a large number of assets.

>[!NOTE]
>
>Before reading this document, you can review the overall [AEM and Creative Cloud integration best practices](https://helpx.adobe.com/experience-manager/6-4/assets/using/aem-cc-integration-best-practices.html) for a higher-level overview of the topic.

## AEM Desktop App architecture {#aem-desktop-app-architecture}

AEM Desktop App uses WebDAV (Windows) or SMB (Mac) network shares to mount network shares. The mounted network share is local only. AEM Desktop App intercepts the calls (open, read, write) and provides additional local caching. It translates remote calls to the AEM Assets server to optimized AEM HTTP requests. The following diagram depicts the AEM Desktop App architecture.

![AEM Desktop app architecture](assets/chlimage_1.png)

The additional caching on write when a file is saved causes the file to be saved locally first (so that the user doesn’t wait for the network transfer). Then, after a predefined delay (  30s ) the file is uploaded to AEM in the background, and then asset is uploaded to AEM. AEM Desktop App provides a UI for monitoring the status of background file uploads.

## Recommended use of AEM Desktop App {#recommended-use-of-aem-desktop-app}

Key capabilities of AEM Desktop App include:

* Opening files from AEM Assets Web UI on desktop: From the web UI, you can reveal assets on desktop (in Finder, Explorer), or open an asset using a desktop application.
* Check-out and check-in: Assets can be checked out for editing, they are marked as locked for the user in AEM Assets. After editing, the asset can be checked in to unlock it.
* Save changes to files: Any change that you save to the file in the network share is uploaded to AEM automatically, and a new version is created. 
* Place linked assets in other documents: In applications, such as Creative Cloud (PS, ID, AI, etc), you can place an external file as a link (for example, you can place an image into an InDesign document). In this case, the network  share  mount lets you browse and select assets from AEM for placement. Placing linked files also works in some non-Adobe apps, such as MS Office.
* Reference resolution in AEM: If both the placed file(s) and the main file with link(s) are stored in AEM, it can automatically provide server-side information about the asset references.
* Access the asset from desktop: In the mounted network share, a contextual menu provides a More Info dialog (larger preview, key metadata) and the ability to open an asset in the AEM UI.
* Uploading large, hierarchical folders in bulk: If you use the Create &gt; Folder Upload option in AEM UI to upload assets, AEM Desktop App uploads the selected folder hierarchy to AEM in the background. Upload progress can be monitored with a dedicated UI in the desktop app.

## Inappropriate use of AEM Desktop App {#inappropriate-use-of-aem-desktop-app}

* Do not use AEM Desktop App to manage assets from the desktop. AEM Desktop App was not built as a replacement for network drives. Use the following capabilities instead:
  * AEM Assets web UI for digital asset management (finding / sharing assets, metadata, copy/move, etc.)
  * AEM Desktop App Folder Upload to upload large, hierarchical folders

* Do not treat AEM Desktop App as a “desktop sync” client for AEM Assets. The key benefit of AEM Desktop App here is that it provides "virtual" access to the whole repository, and desktop sync applications typically synchronize just assets belonging to one user. AEM Desktop App provides some level of caching and background upload; still, it works very differently from typical “Sync” applications, such as Adobe CC Desktop App or Microsoft OneDrive.
* Do not use AEM Desktop App network drives to save assets frequently. All save operations are transmitted to AEM Assets. Therefore, it is impractical to perform intensive edit operations directly in the mounted AEM Assets repository. Editing an asset directly in the mounted repository crams the asset's timeline with irrelevant versions and imposes additional overheads on the server.
* Do not use AEM Desktop App for migration of large amounts of data from one AEM instance to another. Please refer to the [Migration Guide](https://helpx.adobe.com/experience-manager/6-4/assets/using/assets-migration-guide.html) to plan and execute asset migrations. In contrast, Desktop App [supports bulk uploading](use-app-v1.md#bulkupload) large number of assets for the first time in AEM.

## Recommendations for selected use cases {#recommendations-for-selected-use-cases}

### Access to assets for creative users {#access-to-assets-for-creative-users}

AEM Desktop App provides virtual access to the whole DAM repository - and it might be complicated for the creative users on desktop to find and access the right assets on their desktop. Use these best practices to simplify that for them.

* Use collaboration features in AEM Assets Web UI to provide more direct access to the right assets for the creative user. Sharing folders or collections, providing Smart Collections (saved searches), or sending notifications with pointers to the right assets are some of them. Creative user can then use desktop acions in the web UI to quickly get access to these assets on their destkop.
* Consider the right permissions for assets (access control) to simplify the view into the DAM repository for the creative users, basically limiting their access to only assets they need / are interested in:

  * Certain areas not relevant to the creative users might be denied for their user group(s), to remove them from their view, also on desktop
  * Most assets in DAM are final and not intended for changing - these should be read-only for the creative users
  * Only assets that require changes / retouching should be write-enabled for the creative users. Some organizations use AEM Projects and the folders they create to host assets that are still subject to changes.

### Searching assets {#searching-assets}

To search for a file that you want to open on  desktop :

* Use the AEM Assets web UI to locate the asset. Not only  is search  in AEM Assets powerful (search facets, saved searches), it also provides additional capabilities to find the right asset. These include additional filters, like the ability to search assets based on status (approval, expiry), collections, tasks, notifications, and sharing folders/collections with other users/groups.
* After you locate the asset, use Desktop Actions in AEM UI to access the asset on  desktop .

### Updating assets opened using AEM Desktop App {#updating-assets-opened-using-aem-desktop-app}

If you edit an asset directly at the location mapped from AEM Assets to a local network share, the asset is uploaded to AEM each time you save it on desktop. In addition, AEM creates a version and generates renditions.

If an asset stored in AEM needs an update:

* For **minor updates**, such as minor retouching requests in the approval process:

  * Check the file out and open it on desktop
  * Update the file
  * Save the updated version. The asset is updated, and the timeline displays the original version for comparison

* For **major updates**, such as a change request that requires a small creative WIP cycle:

  * Use the Reveal option to open the appropriate folder on desktop
  * Copy the file to a WIP folder **outside** of the mapped AEM Assets share (for example, copy the file into a folder synced with CC Desktop App)
  * Work on the file and save it intermittently. The changes are not saved to AEM Assets
  * After the edits are complete, move, copy, or save the file mapped from AEM to upload it as a new version

## Network performance {#network-performance}

Good experience for users using the AEM Desktop App greatly depends on good, stable network connectivity between their desktops and the AEM server, as well on the server tuned for good performance, especially around uploading and updating assets. These recommendations are for the network / IT teams in organizations.

### Network considerations {#network-considerations}

To understand best practices around AEM Assets network configuration, please refer to [AEM Assets Network Considerations](https://helpx.adobe.com/experience-manager/6-4/assets/using/assets-network-considerations.html) document. Some of the important aspects that help optimize AEM Desktop App experience for the users include:

* **Use properly configured Dispatcher:** Use AEM Dispatcher for additional security and ensure that it is configured for [AEM Desktop App connection to AEM behind a dispatcher](https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html#ConnectingtoAEMBehindaDispatcher)

* **Save bandwidth:** Consider turning off icon preview in Finder on Mac - when browsing the mounted repository using Finder. Finder requests each file to generate a preview and causes desktop app to download & cache the asset locally. Please note that while saving bandwidth it would also decrease user experinece for the users on desktop, so it should be done when working with repositories with large assets and/or limited bandwidth.

**Note:** To turn off icon previews, in Finder go to View, select View Options, and then uncheck the "Show icon preview" option. This only works for the current folder - to make it a default, click the "Use as default" button in the same window.

### Optimizing server performance {#optimizing-server-performance}

To understand, how AEM Assets server should be optimized for performance, please refer to [AEM Assets Performance Tuning Guide](https://helpx.adobe.com/in/experience-manager/6-4/assets/using/performance-tuning-guidelines.html). Some of the important aspects of the server performance for AEM Desktop App are around optimizing workflow configuration so that it performs well for asset uploads:

* **More performant asset upload:** Configure the [AEM Asset Update workflow model to be transient](https://helpx.adobe.com/experience-manager/6-4/assets/using/performance-tuning-guidelines.html#Workflows).

* **Limit server CPU for uploads:** Ensure that the maximum parallel workflow jobs parameter is set  correctly,  so that uploads do not exhaust all the CPU.