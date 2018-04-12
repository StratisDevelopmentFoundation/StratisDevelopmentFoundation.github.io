---
title:  "Add Multisig Endpoint"
categories: bounties coding
post_importance: 2
bounty: 30
---
Earn {{ page.bounty }} STRAT by completing this task.
{: .notice--info}

# Bounty Description

Adding an endpoint to the Stratis Full Node API that lets users create multisig transactions.

See [Adding a Feature](/add_feature/) for info on how to add endpoints to the API.

See the Wallet Feature in the Full Node source code to see how transactions are built. Multisig transactions are part of NBitcoin already, so it would be a matter of extending the Wallet to use the multisig code.