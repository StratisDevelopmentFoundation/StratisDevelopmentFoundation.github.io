---
title:  "OP_RETURN Block Explorer"
categories: coding bounties
post_importance: 2
bounty: 100
---
Earn {{ page.bounty }} STRAT by completing this task.
{: .notice--info}

# Bounty Description

Extending the [Azure Block Explorer](/azure_block_explorer/) to index transactions that have an OP_RETURN code.

Information about OP_RETURNS are stored in a new database and some new endpoints to retrieve OP_RETURNS are added.

Example API queries:

"All transactions containing OP_RETURNS"

"All transactions containing OP_RETURNS since block X"

"All transactions containing OP_RETURNS on address Y"

"All transactions containing OP_RETURNS since block X on address Y"

"All transactions containing OP_RETURNS where the OP_RETURN starts with string 'abc'"

The database columns for where the OP_RETURNs are stored would probably be:

Transaction ID, Block Height, Input Addresses, Output Addresses, OP_RETURN message text, transaction number within the block