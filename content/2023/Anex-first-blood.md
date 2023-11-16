+++
title = 'Anex - First Blood'
date = 2023-11-16T09:50:47+01:00
draft = false
+++

In the past, I have worked on several larger solutions in DotNet - and to get to know Azure, I need something fairly substantial to have it make sense and feel the pain points of the platform.

Introducing Anex (There is a clever backstory here, that I'll leave for later) - at the bottom of this post, I'll link to the release version of the repository that matches my progress.

Microsoft just launched .NET 8.0 (see the [launch announcement](https://devblogs.microsoft.com/dotnet/announcing-dotnet-8/)) - so, naturally, we are going cutting edge and taking all the pains along the way. This includes having to work in Visual Studio, since Rider does not support 8.0 fully yet. The most confusing part was choosing between the templates when creating a new project - I went with "ASP.NET Core Web API", which annoyingly creates something with Weather Forecasts.

So, quick clean-up, removing all that crap and references to it - and we are ready to add the first Controller - the LedgerTagController. For now, it's not really important what a LedgerTag is, I'll do a blog post later about the domain we are working with here - right now, I just need enough to have something show up in Swagger.

    [ApiController]
    [Route("[controller]")]
    public class LedgerTagController : ControllerBase
    {
        private static Dictionary<long, LedgerTagDto> _ledgerTags = new Dictionary<long,LedgerTagDto>
        {
            {1, new LedgerTagDto{Id=1, Description="Telephone costs"}},
            {2, new LedgerTagDto{Id=2, Description="Other costs"} }
        };
        public LedgerTagController()
        {
        }

        [HttpGet()]
        public async Task<IActionResult> Get()
        {
            return Ok(_ledgerTags.Values);
        }

        [HttpGet("{id}")]
        public async Task<IActionResult> GetById(long id)
        {
            return _ledgerTags.TryGetValue(id, out var ledgerTag) 
                ? Ok(ledgerTag) 
                : NotFound();
        }
    }

Very basic stuff, we are running async - we might as well, since it is needed later. And lo and behold - we get the following swagger output when we run it in debug (I will update later, once I figure out how to left-align the image): 
![Screenshot from Swagger](/images/2023/Swagger-screenshot.png)

You can find the release on [Github](https://github.com/goblinhero/Anex/releases/tag/v1).