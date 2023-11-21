+++
title = 'Rant - Azure Portal'
date = 2023-11-21T10:41:02+01:00
draft = false
summary = 'Ranting is healthy - Azure Portal is an abundant source!'
+++

### Context
I am trying to troubleshoot connection issues from my web app to the database.

#### Installing a local PostGres client

This all went fine (it's kind of a mess with how it is structured, but by no means the worst I've seen).

#### Connecting to the Azure database

Here, things went rapidly down-hill. I followed the guide from the Azure Portal:

    Open pgAdmin 4: Launch the pgAdmin 4 application on your computer.
    Register a new server: In the pgAdmin 4 interface, right-click on "Servers" in the left-side browser tree, and select "Register” -> “Server"
    Configure server details: In the "Register - Server" window, you will see multiple tabs - "General", "Connection", "SSL", and others. Fill in the following details:

        General tab
            Name: Provide a name for the connection (e.g., "myAzureFlexInstance").
        Connection tab
            Hostname/address: Enter <url for Azure database>
            Port: Leave the port number as is (default is 5432) if you don't want to connect through pgBouncer. If you are using PgBouncer for connection pooling, change the port number to 6432.
            Maintenance database: Leave the default
            Username: Enter <username left blank for this post>
            Password: Click on the "Save password" checkbox if you want the password to be preserved and enter the corresponding password for the user.

    Save the configuration: Click the "Save" button to save the server registration. pgAdmin 4 will now establish a connection to your Azure Database for PostgreSQL Flexible Server.

This would be fine, if it actually worked - it does not. The pgAdmin client complains: "Unable to connect to server: connection is bad: No such host is known."

Now. There is a lot of security around cloud-based offerings, which is fine. But for all that is holy - explain which steps and options I have to complete.

### Internet to the rescue?

I've worked with this crap for what feels like an eternity - and usually somebody else will have had the same problem and somehow managed to get through and out of the kindness of their heart published the steps they took to resolve the issue. Enter my new best friend: [SQL Shack](https://www.sqlshack.com/accessing-azure-database-for-postgresql-using-pgadmin/). Everything in this guide is sublime - fine background, introduction and clean screenshots. Except for one small detail: The screenshots are all out-of-date. I'm running pgAdmin 4, version 7.8 - they have used version ... something earlier, but still 4-something. However, the changes in pgAdmin are fairly straightforward, so the intent is clear and I can achieve the same in the new UI.

The Azure screenshots, however... It looks nothing like it - menu items have been renamed, re-arranged and look nothing like they did in May 2021. The relevant settings, I need are under the heading "Settings":

    May 2021                    November 2023
    -------------------------   -------------------------
    Connection security
    Connection strings
    Server parameters           Server parameters
    Replication                 Replication
    Active Directory admin
    Pricing tier
    Properties
    Locks                       Locks

Three(!) out of eight menu items are the same. Everything else (including the things I need) are completely different. Nothing is called 'Firewall' (which is what I looking for to allow connections from my workstation). If I use the search box - no results for 'Firewall'. I've clicked every single item under 'Settings' - no where is 'firewall' found.

Dear Microsoft - do you know what that signals to me? That you have no idea where you're going. There's no strategy, just project groups milling around, adding things willy-nilly because 'we can always change it later'. This does not inspire confidence - it inspires uncertainty as in: 'I wonder how long the thing I'm making will keep working'. And the problem does not stop with nice people working for free for you - Microsoft's own guides, tutorials and other resources are similarly out-of-date.

And this is not some esoteric, edge-case problem, I'm struggling with. I'm trying to connect to a database. That's it. A problem that's been around since the world was in black'n'white. It's well-known technology and just because it is hosted in Azure changes nothing around the basic concepts. 

PS! Once I've figured out how to get it to work, I'll post again to show how I managed it. That's how kind I am.