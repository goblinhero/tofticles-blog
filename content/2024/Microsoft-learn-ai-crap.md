+++
title = 'Microsoft Learn AI Crap'
date = 2024-03-01T13:34:38+01:00
draft = false
summary = 'This is fine...'
+++

I'm in the proces of grabbing the AZ-204 certification. As part of that, Microsoft encourages you to use their [Learn](https://learn.microsoft.com) platform.

They do make a point (disclaimer) that they use AI to generate the courses here, but assure us that they are then vetted by a human afterwards. And this is going exactly as you'd expect it to...

I've PDF'ed this particular [gem](https://learn.microsoft.com/en-us/training/modules/develop-for-storage-cdns/4-azure-cdn-libraries-dotnet), since it will (hopefully) be updated at some point.

At the time of writing - it includes this code snippet:

```
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}

```

Now. There are several problems, here. For one, it does not compile. Second, the method body does not reflect the method name. Third, several variables are not declared. And of course, the same problems are present for the next two code snippets...

Apologists will say: "Oh, that is just the fault of the reviewer - they had a bad day - and it is no problem, it will get fixed". The thing is - Microsoft Learn is supposed to be the authorative source of truth.

Dear Microsoft, you are eroding trust in your platform by not spending the necessary time and resources to make absolutely sure, that your material is accurate, to the point and uses best practises. Documentation is paramount for developers to want to use (and trust) your platform - you are failing at this.

I've downloaded the complete page [here](/images/2024/Interact_with_Azure.pdf) for arguments sake.