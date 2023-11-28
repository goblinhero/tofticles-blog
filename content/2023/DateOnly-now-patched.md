+++
title = 'DateOnly Now Patched'
date = 2023-11-28T08:51:49+01:00
draft = false
summary = 'New types, new headaches'
+++

The code for this post can be found [here](https://github.com/goblinhero/Anex/pull/31). It's basically a bugfix for the feature, explainered [here](https://tofticles.dk/2023/fiscal-period-and-dates/).

One thing, I forgot when adding support for DateOnly in the API, is that new types need specific handling in HTTP:PATCH endpoints, since it relies on a Dictionary of JSON values. Luckily, the code is prepared for this exact scenario, so it was a simple matter of adding these two lines:

    new RelayStrategy<JsonElement?, object?>(v => DateOnly.Parse(v!.Value.GetString()!), _ => typeof(T) == typeof(DateOnly)),
    new RelayStrategy<JsonElement?, object?>(v => DateOnly.Parse(v!.Value.GetString()!), _ => typeof(T) == typeof(DateOnly?)),

I haven't really discussed what that `DictionaryHelper` -does-. It is basically responsible for converting whichever type is received from the client (in JSON format) and into the type of the relevant property. Right now, the implementation fails silently if the client sends the wrong format, which is not ideal, but for now it will have to suffice.

The reason that the implementation is the same for properties that are nullable as well as not, is that we handle null-values in an earlier strategy:

    new RelayStrategy<JsonElement?, object?>(_ => default(T), v => !v.HasValue || v.Value.ValueKind == JsonValueKind.Null),

So, these two new strategies only need to handle non-null scenarios. As an aside - `RelayXxx` is what I call classes that only contains boiler-plate where the logic is injected instead, typically with `Func<T>` and `Predicate<T>`.