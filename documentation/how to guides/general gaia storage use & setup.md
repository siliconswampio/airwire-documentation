# General Gaia Storage Use & Setup

## 1. A decentralized storage architecture

### 1.1 What is Gaia?

In short, Gaia Hub is a **decentralized data storage system**. For a completely decentralized application, transactional data is stored on the blockchain and user application data and/or media is stored in Gaia storage. Storing data off of the blockchain ensures that your application can provide users with high performance and high availability for data reads and writes without introducing central trust parties.

### 1.2 Understanding the Gaia architecture

> Blockchains require consensus among large numbers of people, so they can be slow. Additionally, a blockchain is not designed to hold a lot of data. This means using a blockchain for every bit of data a user might write and store is expensive. *For example, imagine if an application were storing every tweet in the chain.*

A Gaia Storage System consists of a *hub service* and *storage resource* on a cloud software provider. The storage provider can be any commercial provider such as Azure, DigitalOcean, Amazon EC2, and so forth. Typically the compute resource and the storage resource reside same cloud vendor, though this is not a requirement. Gaia currently has driver support for S3 and Azure Blob Storage, but the driver model allows for other backend support as well.

Gaia stores data as a simple key-value store. When an identity is created, a corresponding data store is associated with that identity on Gaia. When a user logs into a dApp, the authentication process gives the application the URL of a Gaia hub, which then writes to storage on behalf of that user.


### 1.3 User control

> One of the core concepts of decentralization is that **the user's data is completely under the user's control**. Without meeting this requirement, your application cannot be considered a dApp (*decentralized application*).

A Gaia hub runs as a service which writes to data storage. The storage itself is a **simple key-value store**. The hub service writes to data storage by requiring a valid authentication token from a requestor. Typically, the hub service runs on a compute resource and the storage itself on separate, dedicated storage resource. Typically, both resources belong to the same cloud computing provider.

![Gaia Architecture](https://raw.githubusercontent.com/alacrityio/alacrity-support-documentation/main/developer%20documentation/resources/gaia-storage.png)

Gaia's approach to decentralization focuses on user control of data and its storage. Users can choose a Gaia hub provider. If a user can choose which Gaia hub provider to use, then that choice is all the decentralization required to enable user-controlled applications. Moreover, Gaia a uniform API to access for applications to access that data.

The control of user data lies in the way that user data is accessed. When an application fetches a file `data.txt` for a given user `alice.id`, the lookup will follow these steps:

1. Fetch the `zonefile` for `alice.id`.
2. Read her profile URL from her `zonefile`.
3. Fetch Alice's profile.
4. *Verify* that the profile is signed by `alice.id`'s key
5. Read the `gaiaHubUrl` (e.g. `https://gaia.alice.org/`) out of the profile
6. Fetch the file from `https://gaia.alice.org/data.txt`.

Because `alice.id` has access to her `zonefile`, she can change where her profile is stored. For example, she may do this if the current profile's service provider or storage is compromised. To change where her profile is stored, she changes her Gaia hub URL to another Gaia hub URL. If a user has sufficient compute and storage resources, a user may run their own Gaia Storage System and bypass a commercial Gaia hub provider all together.

> Users with existing identities cannot yet migrate their data from one hub to another.

### 1.4 Unerstand data storage

A Gaia hub stores the written data exactly as given. It offers minimal guarantees about the data. It does not ensure that data is validly formatted, contains valid signatures, or is encrypted. Rather, the design philosophy is that these concerns are client-side concerns.

## 2. Storage hubs overview

### 2.1 Configuration files

You should store a JSON configuration file either in the top-level directory of the hub server. Alternatively, you can specify a file location using the `CONFIG_PATH` environment variable. The following is an example configuration file for Amazon S3:

    {
        "servername": "localhost",
        "port": 4000,
        "driver": "aws",
        "readURL": "https://YOUR_BUCKET_NAME.s3.amazonaws.com/",
        "pageSize": 20,
        "bucket": "YOUR_BUCKET_NAME",
        "awsCredentials": {
            "accessKeyID": "YOUR_ACCESS_KEY",
            "secretAccessKey": "YOUR_SECRET_KEY"
        },
        "argsTransport": {
            "level": "debug",
            "handleExceptions": true,
            "stringify": true,
            "timestamp": true,
            "colorize": false,
            "json": true
        }
    }

You can specify the logging level, the backend driver, the credentials for that backend driver, and the `readURL` of the hub. Typically, this is the URL for the compute resource on the cloud computing provider — where the hub service is running.

### 2.2 Require the correct hub URL

If you enable on the `requireCorrectHubUrl` option in your `config.json` file, your Gaia hub will require that authentication requests correctly include the hubURL they are trying to connect with. Use this option to prevent a malicious Gaia hub from using an authentication token for itself on other Gaia hubs.

By default, the Gaia hub will validate that the supplied URL matches `https://${config.servername}`, but if there are multiple valid URLs for clients to reach the hub at, you can include a list in your `config.json`:

    {
        ....
        servername: "normalserver.com"
        validHubUrls: [ "https://specialserver.com/",
                        "https://legacyurl.info" ]
        ....
    }

### 2.3 The `readURL` parameter

By default, hub drivers return read URLs that point directly at the written content. For example, an S3 driver would return the URL directly to the S3 file. If you configure a CDN or domain to point at that same bucket, you can use the `readURL` parameter to tell the hub that files can be read from a given URL. For example, the `hub.blockstack.org` Gaia Hub is configured to return a read URL that looks like `https://gaia.blockstack.org/hub/`.

> Unset the `readURL` parameter if you do not intend to deploy any caching.

### 2.4 Open vs. private hubs

#### 2.4.1 Open-membership hub

An open-membership storage hub permits writes for *any* address top-level directory. Every request is validated such that write requests must provide valid authentication tokens for that address. Operating in this mode is recommended for service and identity providers who wish to support many different users.

#### 2.4.2 Private-user hub

A private-user hub receives requests for a single user. Requests are controlled via *whitelisting* the addresses allowed to write files. Recall that each application uses a different app- and user-specific address. It follows, to support application storage, your configuration must add to the whitelist each application you wish to use.

Alternatively, the user's client can use the authentication scheme and generate an association token for each app. The user should whitelist her address, and use her associated private key to sign each app's association token. This removes the need to whitelist each application, but with the caveat that the user needs to take care that her association tokens do not get misused.

## 3. Gaia Setup

* [Set up Gaia hub with Alaio](/docs/protocol/5._Alaio_Gaia_Storage.md)
* [Set up Gaia hub with Blockstacks](https://docs.blockstack.org/data-storage/storage-guide)&nbsp;&nbsp;<i class="fas fa-external-link-alt"></i>