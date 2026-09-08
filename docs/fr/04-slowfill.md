# Chapitre 4 — requestSlowFill et executeSlowRelayLeaf : le filet de securite sans relayeur

Si aucun relayeur n'accepte de remplir un depot avant sa `fillDeadline` — par exemple parce que le tarif propose est trop bas ou que la liquidite manque — le systeme prevoit un filet de securite appele « slow fill » qui garantit que le depositeur recevra tout de meme ses fonds, directement depuis le pool de liquidite du `HubPool`, sans intervention d'un relayeur individuel.

`requestSlowFill` (`contracts/spoke-pools/SpokePool.sol`) peut etre appele par n'importe qui pour signaler qu'un depot donne n'a pas ete rempli et devrait etre inclus dans le prochain root bundle en tant que remplissage lent. Le code precise explicitement que les remplissages lents ne sont possibles que si le token d'entree et le token de sortie sont « equivalents », c'est-a-dire qu'ils routent tous deux vers le meme token L1 via les « pool rebalance routes » du `HubPool`.

Cette demande est ensuite integree par le dataworker dans un arbre merkle de remplissages lents, inclus dans le prochain root bundle propose au `HubPool` (chapitre 6). Une fois la fenetre de contestation optimiste ecoulee, le `HubPool` relaie la racine correspondante vers le `SpokePool` concerne, et `executeSlowRelayLeaf` peut alors etre appele par n'importe qui pour effectivement transferer les fonds au destinataire, en fournissant une preuve merkle d'inclusion verifiee par `MerkleLib.verifyV3SlowRelayFulfillment`.

Si un relayeur remplit finalement le depot avant l'execution du remplissage lent, ce dernier devient caduc — le systeme distingue ce cas via l'enum `FillType.ReplacedSlowFill`, qui signale au dataworker que les fonds initialement reserves pour le remplissage lent doivent etre renvoyes du `SpokePool` vers le `HubPool` plutot que distribues.

[Chapitre suivant : le pool de liquidite du HubPool et le taux de change des parts LP](05-hubpool-liquidite.md)
