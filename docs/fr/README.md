# Parcours francais d'Across Protocol — Bridge par intents et relayeurs

Lecture commentee du protocole de transfert cross-chain Across, en francais, un mecanisme par chapitre.
Aucun code n'a ete installe, compile ni execute : ce parcours est purement documentaire.

1. [Presentation d'Across et de son architecture SpokePool/HubPool](01-presentation.md)
2. [depositV3 : le depot sur la chaine source et le hash de relais](02-depositv3.md)
3. [fillRelay : le remplissage instantane par le relayeur et l'exclusivite](03-fillrelay.md)
4. [requestSlowFill et executeSlowRelayLeaf : le filet de securite sans relayeur](04-slowfill.md)
5. [Le pool de liquidite du HubPool et le taux de change des parts LP](05-hubpool-liquidite.md)
6. [proposeRootBundle : la proposition bondee de racines merkle par le dataworker](06-root-bundle.md)
7. [disputeRootBundle : l'arbitrage d'une proposition contestee par l'oracle optimiste UMA](07-dispute-uma.md)
8. [executeRootBundle : la propagation des racines vers les spoke pools via les adapters](08-execute-root-bundle.md)
9. [RelayerRefundLeaf et executeRelayerRefundLeaf : le remboursement des relayeurs](09-relayer-refund.md)
10. [MerkleLib : les preuves merkle et le suivi bitmap des reclamations](10-merklelib.md)
11. [Les chain adapters : l'abstraction des ponts canoniques par chaine](11-chain-adapters.md)
12. [Upgradeabilite UUPS et administration cross-chain du SpokePool](12-upgradeabilite.md)
13. [Limites connues et perimetre de ce parcours](13-limites-et-perimetre.md)
