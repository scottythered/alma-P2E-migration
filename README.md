# Alma P2E Migration Notebook

## TL:DR


## Background
The P2E, or Physical to Electronic, conversion process is a small but important migration process from one library service platform to the Ex Libris Alma platform. "Physical to Electronic" refers to the legacy of MARC records as describers of physical items. During migration, Ex Libris converts MARC records coded as electronic resources into system-designated electronic resource Alma records. This process is handy if your library manages all electronic resources via your bibliographic database, but what about libraries that use, say, external content managers?

Rice University's Fondren Library, for example, employs Ex Libris's SFX link resolver to manage the bulk of our electronic resources which are otherwise stored in a Sirsi Dynix ILS. In similar cases, libraries can insert data into the 035 MARC field, which designates records to skip the P2E process and allowing link resolvers to continue business as usual. There's a catch, though: you need to already know which records, portfolios, or resources are being managed with your link resolver. In our case, explicit matching between our bibliographic database and our SFX holdings would need to done. And while SFX’s metadata is strong, there will always be disconnects between different systems -- especially since SFX's metadata is totally out of our control.

On top of that disconnect, our prep work indicated we would ultimately be migrating 514,000 MARC records that may or may not have a matching SFX record -- or, in some cases, we might have multiple legacy MARC records matching a single SFX record. Could we skip the process entirely and clean it all up after Alma has gone live? Sure, but we likely would not take down our SFX system in the meanwhile, leaving us with an embarrassing number of duplicate records in our now-live Ex Libris Primo discovery layer.

The solution we’ve been working on to prevent these duplicate resources involves python programming, facilitating record searches of tens of thousands -- if not hundreds of thousands -- of our MARC records to our activated SFX titles. We used the *Fuzzy Wuzzy* Python library, originally developed by seatgeek.com to help sell tickets to concerts and sporting events. Fuzzy Wuzzy allows us to match records by title, either perfectly or, in the case of some deprecated MARC records, imperfectly. By limiting to an extremely high confidence score between two titles, we can extend our matches and reduce the number of active SFX titles that need to be manually checked in Sirsi.

In a limited testing environment, we have scored a 93% match rate, or 3,487 MARC records matched to 3,741 active SFX records. Our next steps involve more matching and fine tuning based on the results.
