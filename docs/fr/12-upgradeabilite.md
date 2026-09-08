# Chapitre 12 — Upgradeabilite UUPS et administration cross-chain du SpokePool

Chaque `SpokePool` est deploye comme un proxy UUPS (`UUPSUpgradeable` d'OpenZeppelin) : son adresse reste stable dans le temps mais son code d'implementation peut etre remplace, contrairement aux contrats immuables comme `LBFactory` ou `LBPair` rencontres dans d'autres protocoles de cette bibliotheque. La variable `crossDomainAdmin` stocke l'adresse consideree comme administrateur du proxy, qui doit normalement pointer vers le `HubPool` sur Ethereum.

Chaque implementation de `SpokePool` specifique a une chaine (`Arbitrum_SpokePool.sol`, `Ovm_SpokePool.sol`, `ZkSync_SpokePool.sol`, etc.) definit sa propre logique dans `_requireAdminSender()`, une fonction virtuelle qui verifie que l'appelant courant est bien le message relaye depuis le `HubPool` via le pont natif de cette chaine specifique — par exemple en verifiant l'expediteur d'un message L1-vers-L2 aliasse dans le cas d'Arbitrum, ou l'expediteur enregistre par le `CrossDomainMessenger` dans le cas d'une chaine OP Stack.

Concretement, pour mettre a jour un SpokePool, le proprietaire du `HubPool` appelle une fonction dediee qui envoie, via l'adapter de la chaine cible (chapitre 11), un appel cross-chain a `upgradeTo` sur le proxy distant. Cette « propriete cross-chain » signifie qu'aucune cle privee locale sur la chaine de destination ne peut a elle seule mettre a niveau un SpokePool : seul le proprietaire du `HubPool` sur Ethereum, via le chemin complet du pont natif, en a la capacite.

[Chapitre suivant : limites et perimetre](13-limites-et-perimetre.md)
