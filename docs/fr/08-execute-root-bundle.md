# Chapitre 8 — executeRootBundle : la propagation des racines vers les spoke pools via les adapters

Une fois la periode de `liveness` ecoulee sans contestation (ou une fois le litige UMA tranche en faveur du proposeur), n'importe qui peut appeler `executeRootBundle` (`contracts/hub-pool/HubPool.sol`) pour chaque `PoolRebalanceLeaf` de la proposition, en fournissant sa preuve merkle verifiee par `MerkleLib.verifyPoolRebalance`. Chaque feuille ne peut etre executee qu'une seule fois, suivi via un bitmap de reclamations (`claimedBitMap`, verifie par `MerkleLib.isClaimed1D`) similaire au motif popularise par le Merkle Distributor d'Uniswap.

L'execution ajuste d'abord la comptabilite interne du `HubPool` (`liquidReserves`/`utilizedReserves` du token concerne, chapitre 5), puis delegue l'envoi effectif des fonds et des messages cross-chain a un contrat « adapter » specifique a la chaine de destination, implementant `AdapterInterface` (`contracts/chain-adapters/interfaces/AdapterInterface.sol`) : `relayMessage` pour transmettre un appel arbitraire au `SpokePool` distant (notamment pour lui relayer la racine de remboursement de relayeurs et la racine de remplissages lents via `relayRootBundle`), et `relayTokens` pour faire transiter les `netSendAmounts` par le pont canonique de la chaine cible.

Cette indirection par adapter est ce qui permet a Across de supporter des dizaines de chaines aux mecanismes de pontage radicalement differents (Arbitrum, Optimism/OP Stack, Polygon, zkSync, Linea, etc.) derriere une seule interface commune de deux fonctions, chaque adapter encapsulant les specificites du pont natif de sa chaine.

Une fois que le dernier `netSendAmounts` de la proposition a ete execute (`unclaimedPoolRebalanceLeafCount` tombe a zero), le bond du proposeur lui est automatiquement restitue.

[Chapitre suivant : RelayerRefundLeaf et le remboursement des relayeurs sur le SpokePool](09-relayer-refund.md)
