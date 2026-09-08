# Chapitre 3 — fillRelay : le remplissage instantane par le relayeur et l'exclusivite

`fillRelay` (`contracts/spoke-pools/SpokePool.sol`) est appele sur la chaine de destination par un relayeur qui avance ses propres tokens `outputToken` au `recipient` designe par le depositeur. Le relayeur specifie egalement un `repaymentChainId`, la chaine sur laquelle il souhaite etre rembourse plus tard par le systeme de root bundles (chapitres 6 a 8), ce qui lui permet de consolider ses remboursements sur la chaine ou il gere sa tresorerie plutot que d'etre force de se faire rembourser chaine par chaine.

`_requireExclusiveFiller` verifie que si un `exclusiveRelayer` a ete designe par le depositeur et que l'`exclusivityDeadline` n'est pas encore passee, seul ce relayeur precis peut remplir le depot — une protection qui permet a un relayeur ayant fait une quote agressive de garantir qu'il capturera bien le remplissage sans se faire devancer par un concurrent plus rapide a executer sa transaction.

Le contrat reconstruit un hash de relais a partir des parametres fournis par le relayeur et le compare implicitement au depot original via `getV3RelayHash` : si le relayeur ne fournit pas exactement les memes parametres que ceux emis lors du depot, le hash ne correspondra a aucun depot valide et le remplissage sera considere comme un transfert distinct, non rembourse par le systeme. `fillRelayWithUpdatedDeposit` est une variante qui permet au relayeur d'utiliser un montant de sortie, un destinataire ou un message mis a jour, a condition de fournir une signature EIP-712 du depositeur autorisant explicitement cette modification — utile par exemple si le depositeur souhaite renegocier son tarif apres coup.

Chaque relais ne peut etre rempli qu'une seule fois : le statut de remplissage (`FillStatus`, dans `V3SpokePoolInterface.sol`) passe de `Unfilled` a `Filled`, empechant tout remplissage en double du meme hash de relais.

[Chapitre suivant : le remplissage lent via requestSlowFill et le pool de liquidite](04-slowfill.md)
