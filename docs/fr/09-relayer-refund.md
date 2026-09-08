# Chapitre 9 — RelayerRefundLeaf et executeRelayerRefundLeaf : le remboursement des relayeurs

Une fois que le `HubPool` a relaye la racine de remboursement de relayeurs vers un `SpokePool` donne (chapitre 8), tout relayeur ayant avance des fonds sur cette chaine peut recuperer son remboursement en appelant `executeRelayerRefundLeaf` (`contracts/spoke-pools/SpokePool.sol`) avec une preuve merkle d'inclusion de sa feuille dans cette racine.

Chaque `RelayerRefundLeaf` (`contracts/interfaces/SpokePoolInterface.sol`) contient un tableau parallele `refundAddresses`/`refundAmounts` regroupant potentiellement plusieurs relayeurs a rembourser en une seule feuille pour le meme token L2, ainsi qu'un `amountToReturn` qui, s'il est non nul, indique que le SpokePool doit egalement renvoyer un excedent de fonds vers le `HubPool` via le pont canonique — le miroir exact du `netSendAmounts` negatif correspondant dans le `PoolRebalanceLeaf` du chapitre 6.

Comme pour l'execution des pool rebalance leaves sur le `HubPool`, chaque feuille de remboursement ne peut etre executee qu'une seule fois, suivie par un bitmap de reclamations propre a chaque `SpokePool` et chaque root bundle. Ce decouplage entre le remplissage instantane (chapitre 3, immediat) et le remboursement du relayeur (potentiellement plusieurs heures ou jours plus tard, apres constitution du root bundle et ecoulement de la liveness) est au coeur du modele economique d'Across : le relayeur avance son propre capital et accepte un delai de remboursement en echange des frais captures dans l'ecart entre `inputAmount` et `outputAmount`.

[Chapitre suivant : MerkleLib, les preuves merkle et le suivi des reclamations](10-merklelib.md)
