+++
title = 'DTOs and Convinience Properties'
date = 2023-11-24T14:39:22+01:00
draft = false
summary = 'How to make friends with frontenders.'
+++

The code can be found [here](https://github.com/goblinhero/Anex/pull/29).

So, the focus of this post is the difference between entities and DTOs (Data Transfer Object). The key difference between the two is that the entity is there to support the business ideas and enforce rules - and typically normalized in the database. So, you have complex relationships modelled. DTOs on the other hand is there for the consumer of the API and the rules around them are not near as strict. Typically, they will have little to no logic in them (aside maybe from model validation). And while there is only ever one entity class for a particular business object, there can be many DTOs representing the same entity in different circumstances.

Consider the following classes:

    public class LedgerPostDraft : BaseEntity<LedgerPostDraft>
    {
        protected LedgerPostDraft()
        {
        }

        private LedgerPostDraft(LedgerDraft ledgerDraft)
        {
            LedgerDraft = ledgerDraft;
        }

        public static LedgerPostDraft Create(LedgerDraft ledgerDraft)
        {
            return new LedgerPostDraft(ledgerDraft);
        }
        
        public virtual DateOnly FiscalDate { get; set; }
        public virtual int? VoucherNumber { get; set; }
        public virtual decimal Amount { get; set; }
        public virtual LedgerTag? LedgerTag { get; set; }
        public virtual LedgerTag? ContraTag { get; set; }
        public virtual LedgerDraft LedgerDraft { get; protected set; }
        
        public virtual bool IsReadyForBookkeeping(FiscalPeriod period, out IEnumerable<string> errors)
        {
            return new RuleSet<LedgerPostDraft>(GetBookkeepingRules()).UpholdsRules(this, out errors);
        }
        private IEnumerable<IRule<LedgerPostDraft>> GetBookkeepingRules()
        {
            yield return CannotBeNull(lpd => lpd.LedgerTag);
        }
    }

    public class LedgerPostDraftDto : EntityDto
    {
        [Required]
        public long LedgerDraftId { get; set; }
        [Required]
        public DateOnly FiscalDate { get; set; }
        public int? VoucherNumber { get; set; }
        [Required]
        public decimal Amount { get; set; }
        public long? LedgerTagId { get; set; }
        public int? LedgerTagNumber { get; set; }
        public string? LedgerTagDescription { get;set; }
        public long? ContraTagId { get; set; }
        public int? ContraTagNumber { get; set; }
        public string? ContraTagDescription { get;set; }
    }

    public class EditableLedgerPostDraftDto
    {
        [Required]
        public long LedgerDraftId { get; set; }
        [Required]
        public DateOnly FiscalDate { get; set; }
        public int? VoucherNumber { get; set; }
        [Required]
        public decimal Amount { get; set; }
        public long? LedgerTagId { get; set; }
        public long? ContraTagId { get; set; }
    }

Different problems to solve - different implementations. The focus here is on making the intent clear. The `LedgerPostDraftDto` is designed to be shown in a table or a detail view of the post while `EditableLedgerPostDraftDto` is only there for the communication when editing the entity through the API. As it is evident, the `LedgerPostDraftDto` contains convinience properties - instead of the frontend having to send separate queries to the API to get the number and description on the associated ledger tags, we supply them up front. 

### Why are you not using a mapper framework for the DTOs?

I don't like them. They add another third party dependency for very little benefit in my view. And here, I risk the code resulting in a (up to) SELECT 2N+1 performance problem. (Basically, if I lazy-load 10 LedgerPostDrafts, each of them will try to fetch the associated LedgerTag for the LedgerTag and ContraTag properties resulting in at worst 21 queries to the database which is probably not ideal). Instead, I have made a DB-view that is tailor-made for the DTO and this results in a very clean mapping and no extra roundtrips nor complexity. 

There is no limit to how many different views/DTOs I can have for a single entity, so I could eg. make a view for an auto-complete box with only the `.Id` and `.Description`. At the cost of a few extra classes, which is a cost I am (clearly) more than happy to pay.