---
title:  "Investigate indexing to determine how to add additional indexes"
categories: bounties coding
post_importance: 1
bounty: 10
---
Earn {{ page.bounty }} STRAT by completing this task.
{: .notice--info}

# Investigate indexing to determine how to add additional indexes

Currently only transactions related to the addresses that are contained in the wallet are indexed, as these are the only ones relevant to the operation of the wallet. However, for other use cases, other information about blocks might need to be indexed. As an example, perhaps a developer might want to store information about any transactions that have OP_RETURNS.

Currently blocks are processed as they are received, and some information is stored in a database. This task is to find this part of the code, and investigate what needs to be done extend the database to store more information that can later be retrieved.