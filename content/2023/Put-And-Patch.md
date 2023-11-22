+++
title = 'Put and Patch'
date = 2023-11-22T14:40:34+01:00
draft = false
summary = 'Making third party developers happy!'
+++

You can see my implementation PR [here](https://github.com/goblinhero/Anex/pull/25).

The HTTP RPC supports five keywords - and it is all about intentions:

    GET     - Non-destructive, returns a resource
    POST    - Creates a new resource and perhaps returns the created resource
    PUT     - Replaces a resource and perhaps returns the resource
    PATCH   - Updates single properties for the resource and perhaps returns the updated resource
    DELETE  - Deletes a resource

Where PATCH is the newest. I remember a time where we had only GET and POST to choose from.

### Why would you support both PUT and PATCH for the same resource?

This is all about supporting the users of your API - let's say you are a productive company and your API evolves over time. New properties come along - your integration partner made an integration back in June and really does not want to update it since all the new things have nothing to do with their functionality.

Should they have to update his integration just because you added new (optional) functionality?

### But where's the problem?

We are working in C#, here - and as soon as we publish an endpoint like this:

    [HttpPut("{id}/{version}")]
    public async Task<IActionResult> Update(long id, int version, EditableLedgerTagDto dto)

Our contract is:

    public class EditableLedgerTagDto
    {
        public string? Description { get; set; }
    }

So, for all intents and purposes, we expect the `Description` to be present - if it isn't we fill it out with ... `null` (technically, this is what C# with default serializer settings does). When we then update our contract to look like this:

    public class EditableLedgerTagDto
    {
        public string? Description { get; set; }
        public int? Number { get; set; }
    }

Our poor integration partner will be overriding our values for `Number` with null-values. Not ideal. Enter PATCH - which allows partial updates. The trouble is that C# only sort of has support for dynamic things (we are still in a statically typed world), so our contract looks like this:

    [HttpPatch("{id}/{version}")]
    public async Task<IActionResult> Patch(long id, int version, Dictionary<string, JsonElement> updates)

Which isn't particularly helpful. But when we include both a PATCH and PUT endpoint, we allow the integration developer to see which are possible (PUT) and update the ones they know how to fill (PATCH). Huh. We get to have our cake and eat it too - it is a good day.